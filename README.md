# BP-clear
clc
load C:\Users\ASUS\Desktop\p.txt
load C:\Users\ASUS\Desktop\t.txt
save p.mat
save t.mat
p=p
t=t
[p1,ps]=mapminmax(p)
[t1,ts]=mapminmax(t)
[trainsample.p,valsample.p,testsample.p] =dividerand(p,0.7,0.15,0.15）
[trainsample.t,valsample.t,testsample.t] =dividerand(t,0.7,0.15,0.15)
TF1='tansig';TF2='logsig'
net=newff(p,t,[10],{TF1 TF2},'traingdm')
net.trainParam.epochs = 1000
net.trainParam.goal = 1e-7
net.trainParam.lr = 0.01
net.trainFcn='trainlm'
[net,tr]=train(net,trainsample.p,trainsample.t)
[normtrainoutput,trainPerf]=sim(net,trainsample.p,[],[],trainsample.t)[normvalidateoutput,validatePerf]=sim(net,valsample.p,[],[],valsample.t)
[normtestoutput,testPerf]=sim(net,testsample.p,[],[],testsample.t)
trainoutput=mapminmax('reverse',normtrainoutput,ts)
validateoutput=mapminmax('reverse',normvalidateoutput,ts)
testoutput=mapminmax('reverse',normtestoutput,ts)
trainvalue=mapminmax('reverse',trainsample.t,ts)
validatevalue=mapminmax('reverse',valsample.t,ts)
testvalue=mapminmax('reverse',testsample.t,ts)
errors=trainvalue-trainoutput
figure,plotregression(trainvalue,trainoutput)
figure,plot(1:length(errors),errors,'-b')
figure,hist(errors)
figure,normplot(errors)
