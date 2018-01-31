## 对列表中的文本，按照其中数字排序
l = [ 'ch9.txt', 'ch10.txt', 'ch1.txt', 'ch3.txt', 'ch11.txt' ]
l.sort(key = lambda x:int(re.match('\D+(\d+)\.txt',x).group(1)))
