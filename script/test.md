# test

@IF EXIST "%~dp0\node.exe" \( "%~dp0\node.exe" "%~dp0\test" % _\) ELSE \( node "%~dp0\test" %_ \)

