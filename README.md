
#a  single  spider on douban.com
#coding=utf-8
import urllib2,re
p=file('D:\python\own\poem.txt','r')
l=0
while True:    
    j='%d'%l
    # 读取文件中想要搜索的内容
    query= p.readline()
    query=query.strip()
    if len(query)==0:
        break
    txt='D:\python\own\copy'+j+'.txt'
    f=file(txt,'a+')
    i=0
    m=2
    while i<m:
        # 数字转化为字符串并构造URL
        k='%d'%(i*15)
        url='https://movie.douban.com/subject_search?start='+k+'&search_text='+query+'&cat=1002'
        response=urllib2.urlopen(url)
        html=response.read()
        # 如果是第一次搜索的话，匹配出搜索结果一共有多少页
        if i==0:
            title=re.findall('class="thispage">.*<a.*>(.*)</a><span class="next">',html)
            for each in title:
                m=int(each)
        # 匹配出需要的电影名字，并将其写入到txt文本中
        content = re.findall('from:\'mv_subject_search\'.*title="(.*?)">',html)
        for ev in content:
            f.write(ev)
            f.write("\n")
        i=i+1
    f.close()
    l=l+1
p.close()
