## Overview

This page explains how to understand the cause of problems that may occur when
deal with PMC.

## Error condition

If something goes wrong PMC stops and gives the error code in `pm.fsm_errno`.

	(pmc) reg pm.fsm_errno

## Internal PMC codes

`PM_ERROR_ZERO_DRIFT_FAULT` - Zero drift of current sensor is above the fault
threshold. This may happen if current sensing circuit is broken or inaccurate.

`PM_ERROR_NO_MOTOR_CONNECTED` - Power stages are OK but no machine detected at
the output terminals. Ignore this error if you know that.

`PM_ERROR_BOOTSTRAP_FAULT` - Actual bootstrap retention time is lower that
configured. An extremely low value may indicate that power stages are
completely inoperable.

`PM_ERROR_POWER_STAGE_BROKEN` - No appropriate voltage response detected at
the output terminals. Power stages or terminal voltage sensors are definitely
broken.

`PM_ERROR_LOW_CURRENT_ACCURACY` - Result of current sensor adjustment shows the
deviation is above the fault threshold. Check your current measurement circuit
or increase the fault threshold if you are sure that everything is OK.

`PM_ERROR_LOW_VOLTAGE_ACCURACY` - Result of voltage circuit adjustment shows
the deviation is above the fault threshold. Check your voltage measurement
circuit or increase the fault threshold if you are sure that everything is OK.

`PM_ERROR_LOW_DEADBAND_ACCURACY` - Result of deadband distortion adjustment
shows the deviation is above the fault threshold. If your machine stator
resistance is too high (over ~0.1 Ohm) consider to disable DCU compensation.

`PM_ERROR_CURRENT_LOOP_FAULT` - This means that we apply the full voltage but
actual current is still low. If current sensing circuit is OK this may be
caused by open circuit in machine phases or in its wiring. Also a common cause
of this error is a low level of DC link voltage in combine with high machine
stator resistance.

`PM_ERROR_INSTANT_OVERCURRENT` - Overcurrent accident detected. Check for
current regulation transient and noise level. The most likely reason is an
unstable sensorless estimate causes a current burst.

`PM_ERROR_DC_LINK_OVERVOLTAGE` - DC link overvoltage accident detected. Check
for the machine speed does not rises uncontrolled.

`PM_ERROR_UNCERTAIN_RESULT` - Result of adjustment is uncertain or
ill-conditioned. The observability conditions may not have been met. For
example there was no speed maneuver during the moment of inertia probing.

`PM_ERROR_NAN_OPERATION` - Numerical instability inside PMC control code or
deliberately invalid operation was requested that could result in NaN.

`PM_ERROR_SENSOR_HALL_FAULT` - Forbidden value of Hall code was detected or
result of Hall self-adjustment shows an inadequacy of sensor measurements.

`PM_ERROR_SENSOR_EABI_FAULT` - Result of EABI self-adjustment shows an
inadequacy of sensor measurements.

## Application level

`PM_ERROR_TIMEOUT` - Timeout has occured because of no event happen we waiting
for. This typically happens when PMC cannot reach the speed setpoint in machine
probing procedures.

`PM_ERROR_NO_FLUX_CAUGHT` - Flux observer has not caught the rotor position for
some unknown reason. Maybe speed is too low or spinup was failed at all.

`PM_ERROR_NO_SYNC_FAULT` - Position estimate discrepancy is out of range. This
erorr code can only come from workbench model.

`PM_ERROR_KNOB_SIGNAL_FAULT` - Knob input signal was lost or corrupted. Check
the reliability and noise immunity of wiring.

`PM_ERROR_SPI_DATA_FAULT` - SPI data transfer was failed frequently. Check the
reliability and noise immunity of wiring.

## Arise by hardware

`PM_ERROR_HW_UNMANAGED_IRQ` - PMC control code has not completed its execution
when the next ADC IRQ occurred. Try to decrease PWM frequency or disable some
computationally expensive features.

`PM_ERROR_HW_OVERCURRENT` - Overcurrent accident detected by hardware.

`PM_ERROR_HW_OVERTEMPERATURE` - PCB overheat condition has been reached.
Apparently temperature regulation by current limiting is not functional.

`PM_ERROR_HW_EMERGENCY_STOP` - Emergency situation detected by emergency
endstop signal.

## RAM log

In case of critical system error like a crystal clock failure or watchdog reset
this information will be logged in volatile RAM. You can also find timestamped
PMC error codes in the log. Note that contents of the log is erased when you
remove power from PMC.

To print out and clear the log contents use these commands.

	(pmc) ap_log_flush
	(pmc) ap_log_clean

## See also

Also look into [Integrity Self Test](IntegritySelfTest.md) page in case of you
if you need to check that your hardware is not broken.

