```python
import json
import os

ohead = "head"
odata = "data"
fp = open(ohead)
dicstr = json.load(fp)

patharry = []
ddata = open(odata, 'r')

def dic_process(dicstr, patharry, ddata):
    for key,value in dicstr.items():
        if 'node-notifier' == key:
              continue
        if 'offset' not in value.keys():
            #not a leaf node,add key to patharry
            if key != 'files':
                patharry.append(key)
            dic_process(value, patharry, ddata)
            if key != 'files':
                patharry.pop()
        else:
            #a leaf node, write to file

            newfilesize = value['size']
            newfileoffs = value['offset']
            print("offset to " + str(newfileoffs) + '\t' + key)

            #if int(newfileoffs) < 164342037:
            #      continue
            
            filename = './output/'
            for tmp in patharry:
                filename += tmp + '/'
            if os.path.isdir(filename) == False:
                try:
                    os.makedirs(filename)
                except:
                    pass
            filename += key
            
            fp1 = open(filename, 'w')
            ddata.seek(int(newfileoffs))
            try:
                needwrite = ddata.read(int(newfilesize))
                fp1.write(str(needwrite))
            except:
                pass
            
            fp1.close()
            
dic_process(dicstr, patharry, ddata)

ddata.close()
```
