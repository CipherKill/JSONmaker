#!/usr/bin/python3
import os
from printkit import cprint

dataSeperator=';'
indexMemory=0

def makeJSONTemplate(labels):
	JSON_TEMPLATE='"{}":"{}"'
	RESULT=''
	LIMITER=int(len(labels)-1)
	for i in range(0,len(labels)):
		RESULT=RESULT+JSON_TEMPLATE.format(labels[i],'{}')
		if(i!=LIMITER):
			RESULT=RESULT+','
	return RESULT

def getInitialInputs():
	try:
		cprint.yellow('\n(Use "," as seperators)')
		labelNames="id,"+str(input("Enter the labelnames:~"))
		labelNames=labelNames.replace(' ','').split(',')
		entryCount=int(input("Enter the number of entries:~"))

		cprint.green("[!]Preparing JSON Template for entries....")
		jsonTemplate=makeJSONTemplate(labelNames)
		getEntries(jsonTemplate,entryCount,labelNames)

	except ValueError:
		os.system('clear')
		cprint.red('Invalid Inputs')
		getInitialInputs()

def getEntries(jsonTemplate,entryCount,labelNames):
	try:
		entries=[]
		global filename, dataSeperator, indexMemory
		filter=[dataSeperator+' ',' '+dataSeperator,' '+dataSeperator+' ']
		format_string=''
		for label in labelNames[1:]:
			format_string=format_string+label+dataSeperator
		format_string=format_string[:-1]
		cprint.yellow('\nINPUT FORMAT: {}'.format(format_string))

		for entryIndex in range(indexMemory,entryCount+indexMemory):
			entryCache=str(entryIndex)+';'+str(input("Enter data for entry {}:\n:> ".format(entryIndex+1)))
			for pattern in filter:
				entryCache=entryCache.replace(pattern,dataSeperator)
			if(entryCache[-1]==' '):
				entryCache=entryCache[:-1]
			entries.append(entryCache.split(dataSeperator))

		generate(jsonTemplate,entries)
		cprint.green("\nSuccessfully generated JSON file!\nFile Location:{}/{}".format(os.getcwd(),filename))

	except:
		os.system('clear')
		cprint.red("[!]BAD inputs! try again")
		getEntries(jsonTemplate,entryCount,labelNames)

def generate(jsonTemplate,entries):
	result=''
	mainResult='['
	global filemode
	if(filemode=='a'):
		filereader=open(filename,'r')
		cache=filereader.read()[:-2]+',\n'
		filereader.close()
		mainResult=cache
		filemode='w'
	for entry in entries:
		result=result+jsonTemplate.format(*entry)+','
		result='{'+result[:-1]+'},\n'
		mainResult=mainResult+result
		mainWriteToFile(mainResult)
		result=''
	mainWriteToFile(mainResult[:-2]+']')

def mainWriteToFile(data):
	global filename,filemode
	cache=''
	if(filemode=='a'):
		filereader=open(filename,'r')
		cache=filereader.read()[:-1]
		filereader.close()
		mainResult=cache
		filemode='w'
	mainfile=open(filename,'w')
	mainfile.write(cache)
	mainfile.write(data)
	mainfile.close()

def getMetaInputs():
	filename=str(input('Enter the name of the file:~'))
	cprint.yellow("\nFILE MODES\n[1]Overwrite Mode(first time)\n[2]Append Mode(to add)")
	mode=int(input("Use file mode:~"))
	if(mode==1):
		filemode='w'
	elif(mode==2):
		filemode='a'
	return filename,filemode

def remember():
	try:
		global indexMemory,filename,filemode
		file=open(filename,'r')
		indexMemory=len(file.readlines())
		file.close()
	except:
		cprint.red('[!]Old file not found! Creating new file.')
		filemode='w'
		indexMemory=0

filename,filemode=getMetaInputs()
remember()
getInitialInputs()
