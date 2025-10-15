Quantum Cloud Service  {#tutorial_qcloud_service}
=============================================================

[TOC]

@prev_tutorial{tutorial_quantum_profiling}

Origin Quantum Cloud Service
-------------------------------------------------------------------------------------------------------------------------------

In complex quantum circuit simulations, it is necessary to leverage high-performance computing clusters or real quantum computers. Using cloud computing to replace local computation reduces the computational costs for users and provides a better computing experience.

The Origin Quantum Cloud platform submits tasks to quantum computers or computing clusters deployed remotely via the Origin QPilot service, and receives the returned results. The process is shown in the diagram below.

![Origin Quantum Cloud platform](images/qcloud.png)

pyqpanda3 wraps various quantum computing services, enabling the sending of computation instructions to Origin's computing server clusters or real quantum chips, and retrieving the computation results. For more details, see the diagram below.

![Quantum Cloud Services](images/2024cloud.png)

Users first need to **register and access** the latest Origin Quantum Cloud Computing website at [Origin Quantum Cloud](https://qcloud.originqc.com.cn/).

![Quantum Origin](images/2024origin.png)

Next, click on the **Workbench** in the upper-right corner to enter the quantum computing access interface.

You will see various computing services, including virtual computing and real quantum computing. Then, you need to obtain the **api_token** and other related information. The **api_token** is an identifier for each user to access quantum cloud computing resources via the `pyqpanda3` computing interface. You can obtain it from your personal account center.

![Quantum Origin](images/token.png)

The **api_token** is an important credential for accessing quantum computing resources. Please keep it safe. 

In pyqpanda3, we use [QCloudService](@ref pyqpanda3.qcloud.QCloudService) to build the quantum cloud service and obtain the backend. The api_key of the user needs to be provided.

```python
from pyqpanda3.qcloud import QCloudService
apikey = "XXXXXXXX"
service = QCloudService(apikey)
```

Get your real api token from https://account.originqc.com.cn/, then retrieve all supported backends.

```python
backends = service.backends()
```

The following are some commonly used backends and their corresponding numbers of qubits. 
**Backends with the "amplitude" suffix indicate that they simulate quantum computing using a supercomputing cluster. Other backend names represent real quantum chip backends.**

```python
+----------------------------------------------------------+
|Backend name        |Backend type            | max qubits   
+----------------------------------------------------------+
|full_amplitude      |Simulator               | 33
+----------------------------------------------------------+
|single_amplitude    |Simulator               | 200  
+----------------------------------------------------------+
|partial_amplitude   |Simulator               | 64  
+----------------------------------------------------------+
|origin_wukong       |Physical Quantum Chip   | 72  
-----------------------------------------------------------+
```

Note that the backends are updated periodically at irregular intervals.

Obtain the specific computation backend by backend name.Currently, the chip backend supports the latest origin's Wukong 72-bit chip.

```python
backend = service.backend("origin_wukong")
```

If an error occurs during task execution, you can choose to enable logging to view detailed information.

```python

from pyqpanda3.qcloud import QCloudService, LogOutput

apikey = "XXXXXXXX"
service = QCloudService(apikey)

#print log information to console
service.setup_logging(LogOutput.CONSOLE)

#write log information to log file
service.setup_logging(LogOutput.FILE,"test.log")
```

# High-Performance Computing Cluster Cloud Service

All virtual machines submit quantum computing tasks to the supercomputer via the run method with parameters, for full amplitude.

```python
from pyqpanda3.core import H, CNOT, measure, QProg
from pyqpanda3.qcloud import QCloudService, QCloudOptions,QCloudJob,QCloudResult

prog = QProg()
prog << H(0) << CNOT(0, 1) << measure(0, 0) << measure(1, 1)

api_key = "XXXXXXXXXXXXXXX"
service = QCloudService(api_key=api_key)
backend = service.backend("full_amplitude")
job = backend.run(prog, 1000)
```

[job.job_id](@ref pyqpanda3.qcloud.QCloudJob.job_id) can obtain the task ID 

```python
print("job id : ", job.job_id())

#job id : A45DE13AA1920B115220C19BC0C0F4A5
```

All tasks support both synchronous and asynchronous submission methods, for synchronous...

[job.result](@ref pyqpanda3.qcloud.QCloudJob.result) will block and continuously poll for the task result until the task completes or fails.

```python
    result= job.result()
    probs = result.get_probs()
```

Asynchronous method as above, [job.status](@ref pyqpanda3.qcloud.QCloudJob.status) will query the task status once and return the current status of the task.

```python
import time
from pyqpanda3.qcloud import JobStatus

while True: 
    status = job.status() 
    if status == JobStatus.FINISHED: 
        break 
    time.sleep(5) 
```

In addition to the full amplitude, there are also partial amplitude and single amplitude simulators.

```python
from pyqpanda3.core import QProg,H,CNOT,RX,RY
from pyqpanda3.qcloud import QCloudService

prog = QProg(6)
prog << H(0)
prog << CNOT(1, 2)
prog << RY(0, 0.34)
prog << RX(1, 0.98)

api_key = "302e02010030101010410b6d33ad8772eb9705e844394453a3c8a/6327";

service = QCloudService(api_key)
service.setup_logging()
single_amplitude_backend = service.backend("single_amplitude")

single_amplitude_job = single_amplitude_backend.run(prog, "0")
print(single_amplitude_job.result().get_amplitudes())

partial_amplitude_backend = service.backend("partial_amplitude")
partial_amplitude_job = partial_amplitude_backend.run(prog, ["0","1","2"])

print(partial_amplitude_job.result().get_amplitudes())
```

The output is as follows.

```python
{'0': (0.5093563616148238+0j)}
{'0': (0.5093563616148238+0j), '1': (0.7204633002944995+0j), '2': (0.5093563616148238+0j)}
```

# Real Quantum device

The interface usage for real chips is similar, and the computation backend is obtained based on the backend name.

The run method for submitting computation tasks supports batch tasks.

```python
from pyqpanda3.core import H, CNOT, measure, QProg
from pyqpanda3.qcloud import QCloudService, QCloudOptions

prog = QProg()
prog << H(0) << CNOT(0, 1) << measure(0, 0) << measure(1, 1)

#get your real api token from https://account.originqc.com.cn/
api_key = "302e020100301006d0202010104"
service = QCloudService(api_key=api_key)
backend = service.backend("origin_wukong")
options = QCloudOptions()
job = backend.run([prog, prog], 1000, options)

probs_list = job.result().get_probs_list()

for result in probs_list:
    print(result)
```

The running result is as follows:
```python
{'00': 0.3166741132736206, '01': 0.013481048867106438, '10': 0.06137626618146897, '11': 0.6084685921669006}
{'00': 0.35036635398864746, '01': 0.013929451815783978, '10': 0.06072771921753884, '11': 0.5749766230583191}
```

Asynchronous method as above:

```python
from pyqpanda3.core import H, measure, QProg
from pyqpanda3.qcloud import QCloudService, QCloudOptions, JobStatus
 
prog = QProg()
prog << H(0) << measure(0, 0) << measure(1, 1)
 
#get your real api token from https://account.originqc.com.cn/
api_key = "302e020100d8772eb9705e844394453a3c8a/6327"
service = QCloudService(api_key=api_key)
backend = service.backend("origin_wukong")
options = QCloudOptions()
job = backend.run([prog, prog], 1000, options)
 
import time
while True: 
    status = job.status() 
    if status == JobStatus.FINISHED: 
        break 
    time.sleep(5) 
 
for result in job.result().get_probs_list():
    print(result)
```

The running result is as follows:
```python
{'00': 0.37110814452171326, '01': 0.1298290193080902, '10': 0.36970818042755127, '11': 0.12935470044612885}
{'00': 0.3805724084377289, '01': 0.12036558985710144, '10': 0.3791339993476868, '11': 0.11992792785167694}
```

[QCloudJob](@ref pyqpanda3.qcloud.QCloudJob) can be used to query submitted tasks.

```python
from pyqpanda3.core import H, measure, QProg
from pyqpanda3.qcloud import QCloudService, QCloudOptions, QCloudJob
 
prog = QProg()
prog << H(0) << measure(0, 0) << measure(1, 1)
 
#get your real api token from https://account.originqc.com.cn/
api_key = "302e020100301006072a8648ce3d0201"
service = QCloudService(api_key=api_key)
 
job = QCloudJob("4F01DFBDD46E3571D304FCD4463B9D10")
result = job.result()
 
for result_list in result.get_probs_list():
    print(result_list)
```

The running result is as follows:
```python
{'0x0': 0.5374209880828857, '0x1': 0.4625789225101471}
```

Quantum cloud services also support instruction set tasks. The [to_instruction](@ref pyqpanda3.core.QProg.to_instruction) interface can be used to compile quantum programs into instruction sets supported by the specified backend, and then directly run [run_instruction](@ref pyqpanda3.qcloud.QCloudBackend.run_instruction).


```python
from pyqpanda3.qcloud import QCloudService,QCloudOptions
from pyqpanda3.core import CZ, measure, QProg

prog = QProg()
prog << CZ(9, 10)
prog << CZ(10, 11)
prog.append(measure(9,9))

online_key = "304102010030130602082f8dab977f87edc6547ef4309478deaf1b7e323b6958bc75602462555efa1b3/12457"

service = QCloudService(online_key)
service.setup_logging()
backend = service.backend("WK_C102_400")

options = QCloudOptions()
options.set_amend(False)

ins = prog.to_instruction(backend.chip_backend())
job = backend.run_instruction([ins,ins], 1000, options)

chip_info = backend.chip_info()

result = job.result()
print(result.get_probs_list())
```

The output is:

```python
[{'0x0': 0.7249999761581421, '0x1': 0.2749999940395355}, {'0x0': 0.75, '0x1': 0.25}]
```

[QCloudOptions](@ref pyqpanda3.qcloud.QCloudOptions) can be used to configure runtime options for computation tasks running on the chip, including whether to enable mapping, quantum circuit optimization, and result correction, among others.

```python
from pyqpanda3.qcloud import QCloudOptions
options = QCloudOptions()
options.set_amend(True)
options.set_mapping(True)
options.set_optimization(True)
```

[ChipInfo](@ref pyqpanda3.qcloud.ChipInfo) represents fidelity and other information for each single quantum logic gate and each set of double quantum logic gates

```python
from pyqpanda3.qcloud import QCloudService, LogOutput

api_key = "302e020100301006072a8648ce3d020106052b8104001c0417"

service = QCloudService(api_key)

backend = service.backend("origin_wukong")

chip_info = backend.chip_info()

topo = chip_info.get_chip_topology()

single_qubit_info_list = chip_info.single_qubit_info()
for qubit_info in single_qubit_info_list:
    print(qubit_info)
   
double_qubits_info_list = chip_info.double_qubits_info()
for double_qubits in double_qubits_info_list:
    print("double qubits ",double_qubits.get_qubits(),
          "fidelity : ", double_qubits.get_fidelity()) 

```

Output is :

```python
+---------------------------------------------------------------------------------------------------------------+
|Qubit ID            |Single Gate Fidelity|Readout Fidelity    |Frequency           |T1                  |T2
+----------------------------------------------------------------------------------------------------------------+
|0                   |0.995               |0.9234              |4764                |9.254               |1.128
+----------------------------------------------------------------------------------------------------------------+


+----------------------------------------------------------------------------------------------------------------+
|Qubit ID            |Single Gate Fidelity|Readout Fidelity    |Frequency           |T1                  |T2
+----------------------------------------------------------------------------------------------------------------+
|1                   |0.9973              |0.9417              |4041                |17.436              |1.844
+----------------------------------------------------------------------------------------------------------------+
......

double qubits  [38, 44] fidelity :  0.9831
double qubits  [44, 50] fidelity :  0.0
double qubits  [39, 45] fidelity :  0.0
double qubits  [45, 46] fidelity :  0.9519
double qubits  [45, 51] fidelity :  0.9589
double qubits  [40, 46] fidelity :  0.0
double qubits  [45, 46] fidelity :  0.9519
double qubits  [46, 47] fidelity :  0.0
double qubits  [41, 47] fidelity :  0.964
......

```

1. **Single-Gate** fidelity represents the offset in the error of the result when a single-qubit operation is performed.

2. **Single-Gate** readout error represents the offset in the error when reading the measurement result of the qubit.

3. **Frequency** represents the frequency of single-qubit operations.

4. **T1 and T2** represent the coherence times, measured in microseconds.

5. **Two-qubit fidelity** represents the offset in the error of the result when performing a two-qubit operation.


@note
1.The maximum number of tasks for a batch computation is **200**. If this number is exceeded, the tasks need to be split into multiple submissions.

2.Before using the service, ensure that the user has the necessary permissions and sufficient computing resources, or errors such as "no permission" or "insufficient computing resources" may occur. For more details, refer to [https://qcloud.originqc.com.cn/zh/computerServices](https://qcloud.originqc.com.cn/zh/computerServices).

3.If you encounter any issues while using the service, please submit a [user feedback](https://forum.originqc.com.cn/rostrum/questionIndex.html). We will address your concerns as soon as possible.
