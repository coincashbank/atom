# atom

@echo off

SET EXPECT\_OUTPUT= SET WAIT= SET PSARGS=%\* SET ELECTRON\_ENABLE\_LOGGING= SET ATOM\_ADD= SET ATOM\_NEW\_WINDOW=

FOR %%a IN \(%\*\) DO \( IF /I "%%a"=="-f" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="--foreground" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="-h" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="--help" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="-t" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="--test" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="--benchmark" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="--benchmark-test" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="-v" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="--version" SET EXPECT\_OUTPUT=YES IF /I "%%a"=="--enable-electron-logging" SET ELECTRON\_ENABLE\_LOGGING=YES IF /I "%%a"=="-a" SET ATOM\_ADD=YES IF /I "%%a"=="--add" SET ATOM\_ADD=YES IF /I "%%a"=="-n" SET ATOM\_NEW\_WINDOW=YES IF /I "%%a"=="--new-window" SET ATOM\_NEW\_WINDOW=YES IF /I "%%a"=="-w" \( SET EXPECT\_OUTPUT=YES SET WAIT=YES \) IF /I "%%a"=="--wait" \( SET EXPECT\_OUTPUT=YES SET WAIT=YES \) \)

IF "%ATOM\_ADD%"=="YES" \( IF "%ATOM\_NEW\_WINDOW%"=="YES" \( SET EXPECT\_OUTPUT=YES \) \)

IF "%EXPECT\_OUTPUT%"=="YES" \( IF "%WAIT%"=="YES" \( powershell -noexit "Start-Process -FilePath \"%~dp0....\&lt;%= atomExeName %&gt;\" -ArgumentList \"--pid=$pid $env:PSARGS\" ; wait-event" exit 0 \) ELSE \( "%~dp0....\&lt;%= atomExeName %&gt;" % _\) \) ELSE \( "%~dp0..\app\apm\bin\node.exe" "%~dp0\atom.js" "&lt;%= atomExeName %&gt;" %_ \)

