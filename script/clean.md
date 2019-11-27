# clean

@IF EXIST "%~dp0\node.exe" \( "%~dp0\node.exe" "%~dp0\clean" % _\) ELSE \( node "%~dp0\clean" %_ \)

