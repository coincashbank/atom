# lint

@IF EXIST "%~dp0\node.exe" \( "%~dp0\node.exe" "%~dp0\lint" % _\) ELSE \( node "%~dp0\lint" %_ \)

