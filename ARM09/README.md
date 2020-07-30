# 9 - Demystifying ARM Templates: Loops

## simple loop demo 1:
az deployment group create -g devops-lab-loop-1 -n devopslab-demo1 -f 1-simple-loop.json

## Simple serial loop demo 2:
az deployment group create -g devops-lab-loop-2 -n devopslab-demo2 -f 1-simple-loop-serial.json

## Simple serial batched loop demo 3:
az deployment group create -g devops-lab-loop-3 -n devopslab-demo3 -f 1-simple-loop-batch.json

## Array loop demo 3:
az deployment group create -g devops-lab-loop-4 -n devopslab-demo4 -f array-loop.json


[<-- Episode/ Module 8](../ARM08/README.md) | [Episode/ Module 10 -->](../ARM10/README.md)