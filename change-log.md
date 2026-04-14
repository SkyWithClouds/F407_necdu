# Change Log (CubeMX + Manual Code)

Purpose: Keep a step-by-step record of CubeMX configuration changes and manual code edits to enable reliable rollback.

## Rules
- Add one new entry for every completed CubeMX change step.
- Add one new entry for every manual code change step.
- Keep entries ordered by time (newest at top).
- For manual code changes, include file path, line location, and content summary.

## Entry Template

### [ID] YYYY-MM-DD HH:MM - Step Title
- Type: CubeMX | ManualCode
- Trigger/Issue: 
- Related module: 

#### CubeMX Changes (fill when Type=CubeMX)
- `.ioc` sections changed: 
- Exact settings changed: 
- Regenerated files summary: 
- Notes/Risk: 

#### Manual Code Changes (fill when Type=ManualCode)
- File: 
- Line location: 
- Before summary: 
- After summary: 
- Why this minimal change: 

#### Validation
- Build/Test command: 
- Result: 
- On-board verification: 

#### Rollback Hint
- How to revert this step only: 

---

## Entries

### [006] 2026-03-30 00:00 - Switch ADS1256 self-test to DRDY IRQ + LCD display
- Type: ManualCode
- Trigger/Issue: User changed CubeMX DRDY EXTI to falling edge and requested interrupt-based sampling with on-screen display.
- Related module: Main application flow / ADS1256 BSP

#### Manual Code Changes (fill when Type=ManualCode)
- File: Core/Src/main.c
- Line location: USER CODE PV / USER CODE 2 / while loop
- Before summary: Used ADS1256_ReadAdc(0) polling path and UART printf output.
- After summary: Enabled ScanMode=0 and ADS1256_StartScan(), switched data fetch to ADS1256_GetAdc(0), and rendered ID + CH0 on LCD using lcd_show_string/lcd_fill.
- Why this minimal change: Reuses existing ADS1256 interrupt pipeline and only replaces presentation/output path.

#### Validation
- Build/Test command: CMake Tools build (Debug)
- Result: Pending build after edit.
- On-board verification: Pending.

#### Rollback Hint
- How to revert this step only: Revert Core/Src/main.c interrupt/LCD self-test changes.

---

### [005] 2026-03-30 00:00 - Add ADS1256 to build and fix legacy compile blockers
- Type: ManualCode
- Trigger/Issue: Main integrated ADS1256 self-test but linker failed because ADS1256 source was not part of target; then legacy symbols in ADS1256 source failed compilation.
- Related module: Build system / ADS1256 BSP

#### Manual Code Changes (fill when Type=ManualCode)
- File: CMakeLists.txt
- Line location: target_sources / target_include_directories
- Before summary: ADS1256 source and include path were commented out.
- After summary: Added Drivers/BSP/ADS1256/ads1256.c to sources and Drivers/BSP/ADS1256 to include directories.
- Why this minimal change: Required to link ADS1256 APIs used in main.

#### Manual Code Changes (fill when Type=ManualCode)
- File: Drivers/BSP/ADS1256/ads1256.c
- Line location: includes / globals / ADS1256_WaitDRDY
- Before summary: Included delay.h with incompatible include path; had unused SPI_HandleTypeDef declaration and old LCD_ShowString call.
- After summary: Switched to delay/delay.h, removed unused SPI handle declaration, replaced timeout LCD call with neutral comment.
- Why this minimal change: Fixes compile errors without changing ADC functional path.

#### Validation
- Build/Test command: CMake Tools build (Debug)
- Result: Success, ELF linked.
- On-board verification: Pending.

#### Rollback Hint
- How to revert this step only: Revert CMakeLists.txt ADS1256 source/include additions and related edits in Drivers/BSP/ADS1256/ads1256.c.

---

### [004] 2026-03-30 00:00 - Integrate ADS1256 minimal self-test in main flow
- Type: ManualCode
- Trigger/Issue: Need on-boot ADS1256 basic verification (chip ID + single channel sampling over UART).
- Related module: Main application flow / ADS1256 BSP

#### Manual Code Changes (fill when Type=ManualCode)
- File: Core/Src/main.c
- Line location: USER CODE Includes / PV / USER CODE 2 / while loop
- Before summary: Main loop only displayed LCD hello/counter demo, no ADS1256 initialization or sampling output.
- After summary: Added ADS1256 include and globals, initialized ADS1256 in startup, configured ADC gain/rate, printed chip ID once, and periodically sampled CH0 with UART printf output.
- Why this minimal change: Adds hardware bring-up observability with smallest possible intrusion into existing CubeMX structure.

#### Validation
- Build/Test command: CMake Tools build (Debug)
- Result: Success.
- On-board verification: Pending.

#### Rollback Hint
- How to revert this step only: Revert Core/Src/main.c ADS1256-related additions.

---

### [003] 2026-03-30 00:00 - Let CubeMX own ADS1256 GPIO/EXTI
- Type: ManualCode
- Trigger/Issue: Avoid duplicate GPIO/EXTI configuration between CubeMX and ADS1256 driver.
- Related module: ADS1256 BSP

#### Manual Code Changes (fill when Type=ManualCode)
- File: Drivers/BSP/ADS1256/ads1256.c
- Line location: ADS1256_Init / ADS1256_StartScan / ADS1256_StopScan
- Before summary: ADS1256_Init and scan control functions reconfigured ADS1256 GPIO/EXTI at runtime.
- After summary: Removed GPIO init code from ADS1256_Init; StartScan/StopScan now only enable/disable DRDY NVIC IRQ.
- Why this minimal change: Keeps pin ownership in CubeMX-generated gpio.c and avoids runtime config overwrite.

#### Validation
- Build/Test command: CMake Tools build (Debug)
- Result: Success (no new error from ADS1256 changes).
- On-board verification: Not executed.

#### Rollback Hint
- How to revert this step only: Restore ADS1256_Init/StartScan/StopScan GPIO configuration blocks in Drivers/BSP/ADS1256/ads1256.c.

---

### [002] 2026-03-30 00:00 - ADS1256 board-mapping refactor for pin portability
- Type: ManualCode
- Trigger/Issue: ADS1256 driver still used hard-coded GPIOB/EXTI15_10 naming, not aligned with CubeMX-generated ads1256_* pins.
- Related module: ADS1256 BSP

#### Manual Code Changes (fill when Type=ManualCode)
- File: Drivers/BSP/ADS1256/ads1256.h
- Line location: board mapping macros and pin operation macros section
- Before summary: Directly operated GPIOB PB10-PB15 in pin macros and depended on legacy naming.
- After summary: Added board-mapping macro layer (Pin/GPIO_Port/IRQ/clock-enable), mapped defaults to CubeMX ads1256_* defines, and rewired all ADS1256_* read/write macros to mapped symbols.
- Why this minimal change: Keeps original driver behavior while making board migration possible by editing only macros in one place.

#### Manual Code Changes (fill when Type=ManualCode)
- File: Drivers/BSP/ADS1256/ads1256.c
- Line location: ADS1256_Init / ADS1256_StartScan / ADS1256_StopScan / HAL_GPIO_EXTI_Callback
- Before summary: Initialization and interrupt control were hard-coded to GPIOB and EXTI15_10, with local EXTI15_10 IRQ handler in driver.
- After summary: Replaced hard-coded GPIO/IRQ usage with mapping macros, initialized each pin by its mapped port, switched NVIC control to ADS1256_DRDY_EXTI_IRQn, removed local EXTI15_10 IRQ handler, and updated callback pin compare to ADS1256_DRDY_Pin.
- Why this minimal change: Avoids conflicts with CubeMX-generated interrupt handlers and keeps migration limited to macro edits.

#### Validation
- Build/Test command: CMake Tools build (Debug)
- Result: Build succeeded, ELF/HEX generated.
- On-board verification: Not executed.

#### Rollback Hint
- How to revert this step only: Revert changes in Drivers/BSP/ADS1256/ads1256.h and Drivers/BSP/ADS1256/ads1256.c to previous commit.

---

### [001] 2026-03-29 00:00 - Initialize change log
- Type: ManualCode
- Trigger/Issue: Create mandatory tracking file for CubeMX and manual code changes.
- Related module: Project process

#### Manual Code Changes (fill when Type=ManualCode)
- File: Core/MD/change-log.md
- Line location: 1
- Before summary: File did not exist.
- After summary: Added structured log template and rules.
- Why this minimal change: Needed a single canonical file for rollback-oriented tracking.

#### Validation
- Build/Test command: N/A
- Result: N/A
- On-board verification: N/A

#### Rollback Hint
- How to revert this step only: Remove Core/MD/change-log.md.
