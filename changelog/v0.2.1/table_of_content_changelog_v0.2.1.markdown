v0.2.1 {#changelog_0_2_1}
===========================

## New features and important updates
- **Optimization of quantum cloud computing service interfaces.**

1.The quantum cloud computing chip task has added the point_label option, which allows tasks to be submitted to specified chip area labels

2.The QResult class for quantum cloud computing task results has added an interface to return raw JSON data

Here is an example of the changes in code:

```python
from pyqpanda3.core import H, measure, QProg
from pyqpanda3.qcloud import QCloudService, QCloudOptions, JobStatus
 
prog = QProg()
prog << H(0) << measure(0, 0) << measure(1, 1)
 
#get your real api token from https://account.originqc.com.cn/
api_key = "3041020100301306042730309478de"
service = QCloudService(api_key=api_key)
backend = service.backend("WK_C102_400")
options = QCloudOptions()
options.set_point_label(2)
job = backend.run(prog, 1000, options)
 
import time
while True: 
    status = job.status() 
    if status == JobStatus.FINISHED: 
        break 
    time.sleep(5) 
 
result = job.result()
print(result.origin_data())
 
for result_list in result.get_probs_list():
    print(result_list)
```

The output is as follows:

```python

# full origin data
{"success":true,"code":10000,"message":"success","obj":{"taskId":"FF85F6804299B7ED403888E0EBA7F4C1","pilotTaskId":"31683BBD11B4486A9CA8C24CBB9663CB","errCode":0,"startTime":1745327244015,"taskState":"3","convertQProg":["[{\"RPhi\":[3,270.0,90.0,0]},{\"Measure\":[[25,3],30]}]"],"mappingQProg":["QINIT 25\nCREG 2\nU3 q[2],(1.5707963267949,-3.14159265358979,3.14159265358979)\nMEASURE q[2], c[0]\nMEASURE q[24], c[1]\n"],"mappingQubit":["{SrcQubits:[0],TargetCbits:[1,2],MappingQubits:[2]}"],"measureQubitSize":[2],"aioTimeStamp":"9:1745327247251;8:1745327245103;7:1745327244086;2:1745327245577;","requiredCore":"0","taskType":"0","taskResult":["{\"key\":[\"0x0\",\"0x1\",\"0x2\",\"0x3\"],\"value\":[0.014375101774930954,0.980093777179718,0.00015337103104684502,0.005377839785069227]}"],"aioExecuteTime":3179,"queueTime":210,"compileTime":4,"amendTime":18,"totalTime":3407,"aioCompileTime":805,"aioPendingTime":1751,"aioMeasureTime":160,"aioPostProcessTime":48,"pulseTime":30.0,"cirExecuteTime":100000.0,"QMachineType":null}}

# job result
{'00': 0.014375101774930954, '01': 0.980093777179718, '10': 0.00015337103104684502, '11': 0.005377839785069227}

```

