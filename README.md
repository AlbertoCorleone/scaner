import urllib2
import re
import sys
import threading
threads = 10

targeturl = "https://www.santanderforintermediaries.co.uk/"#sys.argv[1]
urls = [targeturl]
checked = []
filter = [".mp4", ".png", ".jpg", ".gif", ".css"]
j = 0
for u in urls:
    if u not in checked and u[-4:] not in filter:
        print("~~~ path: "+str(j)+" ~~~\nURLS LEN = "+str(len(urls)))
        print("Checking URL: "+u+"\n")
        try:
            request = urllib2.Request(u)
            response = urllib2.urlopen(request)
            content = response.read()
            if(response.code != 200):
                continue
            relative_urls = re.findall(r'href=\"/\S+\"|src=\"/\S+\"', content)
            absolute_urls = re.findall(r'href=\"'+targeturl+'\S+\"|src=\"'+targeturl+'\S+\"', content)
            temp_urls = relative_urls + absolute_urls
            temp_urls = list(set(temp_urls))
            for (i, item) in enumerate(temp_urls):
                '''if "//" in temp_urls[i]:
                    del temp_urls[i]    
                    continue'''
                temp_urls[i] = temp_urls[i].replace('href="/', '')
                temp_urls[i] = temp_urls[i].replace('src="/', '')
                temp_urls[i] = temp_urls[i].replace('href="', '')
                temp_urls[i] = temp_urls[i].replace('src="', '')            
                temp_urls[i] = temp_urls[i].replace('"', '')
                temp_urls[i] = targeturl+temp_urls[i]
            for tu in temp_urls:
                print("tu="+tu+"\n")
                if tu not in urls:
                    urls.append(tu)
            checked.append(u)
        except:
            pass
        j=j+1
for (i, item) in enumerate(urls):
    print(" "+item+"\n")
    
