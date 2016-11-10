import os
import sys
import re
from os.path import splitext,basename

sys.path.append("/utils")
import utils

CONFIG = os.environ["CONFIG"]
INPUT = os.environ["INPUT"] # input folder mounted /input ro?
OUTPUT = os.environ["OUTPUT"] # output  /ouput rw
MSDIR = os.environ["MSDIR"] #           /msdir   WHERE ALL THE MS SHOULD BE


jdict = dict(override_with_parset=False,ndppp_task="count", Input_column= "DATA",Output_column= "DATA")

jdict.update( utils.readJson(CONFIG) )

print jdict

# TESTING OWNERSHIP OF CREATED DATA
os.system("touch "+OUTPUT+"/test.txt")

if not(jdict["override_with_parset"]):  # then generate PARSET FILE
    PARSETFILE="default.parset"
    f=open(INPUT+"/"+PARSETFILE,"w+")
# Common header for every parsets
    f.write('msin = '+MSDIR+"/"+jdict["MS"]+"\n")
    f.write('msin.datacolumn = '+jdict["Input_column"]+"\n")
    f.write('msout = '+OUTPUT+"/"+splitext(basename(jdict['MS']))[0]+".aoflag.MS"+"\n")
    f.write('msout.datacolumn = '+jdict["Output_column"]+"\n")

    f.write('steps = ['+jdict['ndppp_task']+']\n') # one task for the moment

    if jdict['ndppp_task']=="count":
        print "Running Count task with NDPPP"

#    elif jdict['ndppp_task']=="aoflag":
#        print "Running aoflagger task with NDPPP"
#        f.write(jdict['ndppp_task']+".type=aoflag\n")
#        f.write(jdict['ndppp_task']+".memoryperc=25\n")
    f.close()
else:
    PARSETFILE=jdict["override_with_parset"]

utils.xrun("cp", [INPUT+"/"+PARSETFILE,OUTPUT]) # saving a copy of the parset file
utils.xrun("NDPPP", [INPUT+"/"+PARSETFILE])
