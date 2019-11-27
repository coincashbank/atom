# bootstrap

@IF EXIST "%~dp0\node.exe" \( "%~dp0\node.exe" "%~dp0\bootstrap" % _\) ELSE \( node "%~dp0\bootstrap" %_ \)

