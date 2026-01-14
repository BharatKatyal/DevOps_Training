

Limit of 1CPU - it will be limited to contain only 1 CPU

You can set a limit of lets say 512MB Limit 

Incase of CPU it trottles the cpu so it does not go beyond limit

However thats not the case with Memory

It can consume more memory than the limit but the pod will be terminated 

Without resource requests and limits set a pod can use all the resources and sufficte the rest of pods



Limit ranges for Pods are set at the namespace level
They set a range for pods that dont have resource limits set

You can create a yaml for Limit-Range-Memory.yaml and also one for Limit-Range-CPU.yaml

Resourse Quote at a namespace level
