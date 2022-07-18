#!/usr/bin/env python3

# main - Script to download Telegram images.

import sys,requests,bs4,re


def downloader(number):
    global timg
    for i in range(number):
        res = requests.get(timg[i])
        try:
            res.raise_for_status()
        except Exception as exc:
            print('Error %s '%(exc))
            continue
        file1 = open('img'+str(i)+'.jpg','wb')
        for chuck in res.iter_content(100):
            file1.write(chuck)
        file1.close()

def processor(style_value):
    Url=[]
    regex = re.compile(r'https://.*\.jpg')
    for url in style_value:
        mo = regex.search(url)
        if mo == None:
            continue
        Url.append(mo.group())
    return Url        

def srcimg(channelname):
    respObj = requests.get("https://t.me/s/"+channelname)
    try:
        respObj.raise_for_status()
    except Exception as exc:
        print('Error %s ' %(exc))
        sys.exit()
    soupObj = bs4.BeautifulSoup(respObj.text,features='lxml')
    src=[]
    tagObjs = soupObj.select('a[style]')
    for tagObj in tagObjs:
        src.append(tagObj.get('style'))
    return src # list of style's attribute values.    

Option=['-C','-h']
Option2='-N'
helper='''
Options                             Descriptions

-h                                  Display's this help message

-C                                  Name of the Channal

-N[Number]                          Number of images to Download(default:All)

E.g  ./teldown -C WallpapersETdaily 
     ./teldown -C WallpapersETdaily -N 3
     
'''
if len(sys.argv) < 2:
    print('teldown: error: no arguments')
    sys.exit()
elif len(sys.argv) == 2:
    if sys.argv[1] == Option[1]:
        print(helper)
        sys.exit()
    else:
        print('teldown: error: unrecoginzed arguments:'+' '.join(sys.argv[1:]))
        sys.exit()
elif len(sys.argv) < 6:
    if sys.argv[1] == Option[0]:# Channal op
        if len(sys.argv) ==3:
            style_attribute = srcimg(sys.argv[2])
            timg= processor(style_attribute)
            print('Found('+str(len(timg))+'/'+str(len(style_attribute))+')')
            downloader(len(timg))
        elif sys.argv[3] == Option2 :
            if len(sys.argv) > 4:
                style_attribute = srcimg(sys.argv[2])
                timg = processor(style_attribute)
                print('Found('+str(len(timg))+'/'+str(len(style_attribute))+')')
                downloader(int(sys.argv[4]))
            else:
                print('teldown: error: unrecoginzed arguments:'+' '.join(sys.argv[1:]))
                sys.exit()
        else:
            print('teldown: error: unrecoginzed arguments:'+' '.join(sys.argv[1:]))
            sys.exit()

else:
    print('teldown: error: unrecognized arguments:'+' '.join(sys.argv[1:]))
    sys.exit()

