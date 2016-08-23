# [tsdb2csv project link](https://github.com/Jungmo/tsdb2csv)

## TSDB Server에서 값 가지고 오기 & JSON 파싱

~~~~{.python}
def __init__(self, url):
        response = urllib.urlopen(url);
        resp_json = response.read()
        resp_json = resp_json[1:-1]
        json_data = json.loads(resp_json)

        self.metric_name = json_data["metric"]
        self.value = json_data["dps"]
        self.value = func.dict_sort(self.value)
~~~~

* URL : http://10.0.1.43:4242/api/query?start=[시작시간]&end=[종료시간]&m=sum:[Metric]
* 위 URL로 response = urllib.urlopen(url)를 하면 파일과 같은 객체가 반환된다.
  * *If all went well, a file-like object is returned.*
* 즉, 바로 문자열로 읽을 수 없고 response.read() 등으로 문자열로 만들어주어야한다.
* 위 결과는 *이유는 모르겠지만,* 결과값이 []로 둘러쌓여 있어 json 파싱이 안된다.
* json.loads는 JSON 포맷 문자열을 파라미터로 갖는 json parser다.
* json_data[key이름]으로 JSON 내 값을 가지고 올 수 있다. 
* 키 dps를 받는 value는 timestamp와 value를 가지고 있는 dictionary 자료형이다.
  * Order Dictionary by Key
  
    ~~~~{.python}
    od = collections.OrderedDict(sorted(dict.items()))
    ~~~~
    
## Timestamp 변환

~~~~{.python}
def realtime2unixtime(realtime):
    ts = time.mktime(datetime.datetime.strptime(realtime, "%Y/%m/%d-%H:%M:%S").timetuple())
    return ts

def unixtime2realtime(unixtime):
    ts =  datetime.datetime.fromtimestamp(int(unixtime)).strftime('%Y/%m/%d-%H:%M:%S')
    return ts
~~~~

## CSV 작성
~~~~{.python}
import csv

...

f = open('../output/file.csv','wb')
writer = csv.writer(f, delimiter=',')
writer.writerow(['unix_ts','real_ts']+metric)
for i in range(int(mints), int(maxts)):
    ts = str(i).decode("utf-8")
    try:
        writer.writerow([ts, func.unixtime2realtime(ts), hr.value[ts], acc_x.value[ts], acc_y.value[ts], acc_z.value[ts]])
    except KeyError:
        print "error"
~~~~
* writer = csv.writer(f, delimiter=',')로 comma로 데이터를 나눌 것이라고 명시한다.
* writer.writerow() 는 **하나** 의 파라미터를 받는다.
  * 그래서 여러 줄을 넣을 때는 , ['unix_ts','real_ts']+metric 등으로 해야한다.
* 현재 각 인스턴스의 value는 dictionary인데 키가 숫자다.(unix Timestamp) 
  * 숫자는 키가 될 수 없기 때문에 자동으로 Unicode화 되어 쓰여진다.
  * 그래서 읽을 때도 똑같이 Unicode로 읽어야한다.
 
