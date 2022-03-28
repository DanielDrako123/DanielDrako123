@ ECO  APAGADO
REM PCSX2 - Emulador de PS2 para PC
REM Copyright (C) 2002-2015 Equipo de desarrollo de PCSX2
movimiento rápido del ojo
REM PCSX2 es software libre: puede redistribuirlo y/o modificarlo bajo los términos
REM de GNU Lesser General Public License según lo publicado por Free Software Found-
REM , ya sea la versión 3 de la Licencia o (a su elección) cualquier versión posterior.
movimiento rápido del ojo
REM PCSX2 se distribuye con la esperanza de que sea útil, pero SIN NINGUNA GARANTÍA;
REM sin siquiera la garantía implícita de COMERCIABILIDAD o IDONEIDAD PARA UN PARTICULAR
PROPÓSITO REM . Consulte la Licencia pública general de GNU para obtener más detalles.
movimiento rápido del ojo
REM Debería haber recibido una copia de la Licencia pública general de GNU junto con PCSX2.
REM Si no, consulte < http://www.gnu.org/licenses/ > .

CLS
ECHO Seleccione su versión de Visual Studio:
ECO 1. Microsoft Visual Studio 2019
ECHO P. Salga del guión.
OPCIÓN /C 1Q /T 10 /D 1 /M " Versión de Visual Studio: "
SI  NIVEL DE ERROR  2  IR A FIN
SI EL NIVEL DE  ERROR 1 ESTABLECE " VCVARPATH  = %VS160COMNTOOLS% ..\..\VC\Auxiliary\Build\vcvarsall.bat "  

ECO .
ECHO Seleccione la configuración deseada:
ECHO 1. Liberar 32 bits (predeterminado)
ECO 2. Desarrollar 32 bits
ECO 3. Depuración de 32 bits
ECHO 4. Lanzamiento de 64 bits (WIP)
ECHO 5. Desarrollo de 64 bits (WIP)
ECO 6. Depuración de 64 bits (WIP)
ECHO P. Salga del guión.
ELECCIÓN /C 123456Q /T 10 /D 1 /M " Configuración: "
SI  NIVEL DE ERROR  7  IR A FIN
IF  ERRORLEVEL  6  SET  " SELARCH = x64 "  &&  SET  " SELCONF = DebugAll "
IF  ERRORLEVEL  5  SET  " SELARCH = x64 "  &&  SET  " SELCONF = DevelAll "
IF  ERRORLEVEL  4  SET  " SELARCH = x64 "  &&  SET  " SELCONF = ReleaseAll "
SI NIVEL DE  ERROR 3 SET " SELARCH  = x86 " && SET " SELCONF = DebugAll "     
IF  ERRORLEVEL  2  SET  " SELARCH = x86 "  &&  SET  " SELCONF = DevelAll "
IF  ERRORLEVEL  1  SET  " SELARCH = x86 "  &&  SET  " SELCONF = ReleaseAll "

SI  EXISTE  " %VCVARPATH% " (llame a " %VCVARPATH% "  %SELARCH% ) DE LO  CONTRARIO VAYA A ERRORES
cl >  NUL  2 >& 1
si  %ERRORLEVEL%  NEQ  0  GOTO ERRORVS

ECO .
ECO usando:
cl 2 >& 1 |  findstr  " Versión "
ECO .

CONJUNTO  Plataforma =
SET  " LOGOPTIONS = /v:m /fl1 /fl2 /flp1:logfile= " %~dpn0 -%SELARCH%-%SELCONF%-errors.log";errorsonly /flp2:logfile = " %~dpn0 - %SELARCH% - %SELCONF% -advertencias.log " ; soloadvertencias "
msbuild " %~dp0 \buildbot.xml " /m %LOGOPTIONS% /t: %SELCONF%
IR AL FIN

: ERRORES
ECO .
ECHO No se encontró la versión de Visual Studio seleccionada.

: FIN
ECO .
ECO ¡Adiós!
ECO .
tiempo de espera / t 10
