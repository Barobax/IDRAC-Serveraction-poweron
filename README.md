# IDRAC-Serveraction-powerup
check status of the server if server is offline auto powerup.
use at your own risk.
script comes with no support or warranty.


@echo off
title IDRAC AUTO POWER ON
mode con: cols=40 lines=5
COLOR 0a
rem this script comes with no warranty and no support
rem use at your own risk.
:racadmcli
Title Checking for Racadm CLI
if exist "C:\program files\Dell\sysmgt" (
    goto login
) else (
    start http://www.dell.com/support/home/us/en/04/Drivers/DriversDetails?driverId=6RWMR
)
echo please Install Racadm Cli
pause
goto racadmcli
:login
cls
Title Log Into IDRAC
set /p IP="Enter IDRAC IP: "
set /p user="Enter UserName: "
set /p psw="Enter Password: "
:start
Title Checking Power Status
racadm -r %IP% -u %user% -p %psw% --nocertwarn serveraction powerstatus > C:\temp\idrac.log
findstr "ON" C:\temp\idrac.log 
if %ERRORLEVEL% EQU 0 goto Serveron
if %ERRORLEVEL% EQU 1 goto serveroff
goto login
:serveroff
Title Server Status is Off
msg * "The Server Is Powered off"
racadm -r %IP% -u %user% -p %psw% --nocertwarn serveraction powerup
goto powerup
:powerup
racadm -r %IP% -u %user% -p %psw% --nocertwarn serveraction powerstatus > C:\temp\idrac.log
set /p mytitle=<C:\temp\idrac.log
title=%mytitle%
timeout /t 3
goto login
:Serveron
racadm -r %IP% -u %user% -p %psw% --nocertwarn serveraction powerstatus > C:\temp\idrac.log
set /p mytitle=<C:\temp\idrac.log
title=%mytitle%
timeout /t 3
goto login
