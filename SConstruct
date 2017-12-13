import subprocess
import os

ROOTCFLAGS    	= subprocess.check_output( ['root-config',  '--cflags'] ).rstrip().split( " " )

ROOT_SYS 		= os.environ[ "ROOTSYS" ]

cppDefines 		= {}
cppFlags 		= ['-Wall' ]#, '-Werror']
cxxFlags 		= ['-std=c++11' ]
cxxFlags.extend( ROOTCFLAGS )

paths 			= [ '.',  ROOT_SYS + "/include", "./modules/FemtoDstFormat" ]

########################### ROOT dictionary creation ##########################
LD_LIBRARY_PATH = os.environ.get( "LD_LIBRARY_PATH", "" )

rootcint_env = Environment(ENV = os.environ )
# {'PATH' : os.environ['PATH'], 'ROOTSYS' : os.environ[ "ROOTSYS" ], 'LD_LIBRARY_PATH' : LD_LIBRARY_PATH })
rootcint = Builder( action='rootcint -f $TARGET -c $_CPPINCFLAGS $SOURCES.file' )  
rootcint_env.Append( BUILDERS 		= { 'RootCint' : rootcint } )

headers = [ "FemtoPair.h" ]

# hack to make the rootcint use abs path to headers
rootcint_env[ "_CPPINCFLAGS" ] = "-I" + Dir(".").abspath + "/" + " -I" + Dir("..").abspath + "/"
# print rootcint_env[ "_CPPINCFLAGS" ]
root_dict = rootcint_env.RootCint( "DictionaryFemtoPair.cpp", headers )
Clean( root_dict, "DictionaryFemtoPair.h" )
rootcint_env.Alias( 'dict', root_dict )
