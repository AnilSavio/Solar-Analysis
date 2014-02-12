Solar-Analysis
==============
import numpy as np
import os
import fnmatch
import fileinput
import pandas as pd
from pandas import *
from pandas import DataFrame
import glob
import fnmatch
import matplotlib.pyplot as plt
import datetime as dt
from datetime import datetime
from dateutil.parser import parse


class getdata:
    def __init__(self, rootdir, year, month, day, hour,row, column):
        self.rootdir=rootdir
        self.year=year
        self.month=month
        self.day=day
        self.hour=hour
        rootdir ='E:\\time_series_hourly_dni\\'

        searchFileName = 'solar_dni_'+year+month+day+'_'+hour+'UT.txt'
        
        for dirName, subdirList, fileList in os.walk(rootdir):
            if searchFileName in  fileList :
                filePath=os.path.join(dirName,searchFileName)

                print filePath

                DNI=pd.read_csv(filePath,skiprows=6, sep='\s+')
                print DNI.ix[row, column]
                print DNI.ix[200,200]

                with open(filePath) as infile:
                    numberofColumns = int(infile.readline().split(' ')[1].strip())
                    numberofRows = int(infile.readline().split(' ')[1].strip())
                    XLLCorner = float(infile.readline().split(' ')[1].strip())
                    YLLCorner = float(infile.readline().split(' ')[1].strip())
                    cellSize = float(infile.readline().split(' ')[1].strip())
                    nullValue = infile.readline().split(' ')[1].strip()

                  #  if column < numberofColumns and row < numberofRows:
                  #      newCalcCol = column * cellSize  + XLLCorner
                  #      newRowCalc =YLLCorner+ row * cellSize
                  #      coOrdinates= {newCalcCol,newRowCalc}
                  #      print coOrdinates
                  #  else:
                  #      print ('Error column or row is out of index')

                    with open(rootdir+'Matrix.txt','w') as matrixFile:
                        matrixFile.write("month,day,year,hour,coordinate1,coordinate2\n")
                        for r in xrange(0, numberofColumns  -1):
                            for c in xrange(0,  numberofRows -1) :
                                newCalcCol = c * cellSize  + XLLCorner
                                newRowCalc = r * cellSize + YLLCorner
                                matrixFile.write('%s,%s,%s,%s,%s,%s\n' %(month,day,year,hour,newCalcCol,newRowCalc))
                               
                    matrixFile.close()
                    infile.close()
