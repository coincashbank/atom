# postprocess-junit-results

@IF EXIST "%~dp0\node.exe" \( "%~dp0\node.exe" "%~dp0\postprocess-junit-results" % _\) ELSE \( node "%~dp0\postprocess-junit-results" %_ \)

