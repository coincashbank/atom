# build

@IF EXIST "%~dp0\node.exe" \( "%~dp0\node.exe" "%~dp0\build" % _\) ELSE \( node "%~dp0\build" %_ \)

