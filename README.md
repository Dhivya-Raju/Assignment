# Assignment
Docker and kubernetes assignment
1st ► VOTE pod ► Observe what happens both in frontEnd & in Unix
In the Unix vote pod has been deleted and it has recreated immediately with different container ID and Ip also changed

In front end of vote page the container ID of the vote pod is changed to container ID vote-7d884dd585-7hbl6 and didnot observe any behaviour change in vote and result page.
 	
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications# kubectl get po
NAME                      READY   STATUS    RESTARTS   AGE
db-58cc845644-4c7rk       1/1     Running   0          28h
mongodb                   1/1     Running   0          26h
redis-6878558678-5pp4v    1/1     Running   0          28h
result-86bc6f7b5d-nr7kc   1/1     Running   0          28h
vote-7d884dd585-86dbn     1/1     Running   0          28h
worker-6fc5d5b668-6xstl   1/1     Running   0          28h
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications# kubectl delete po vote-7d884dd585-86dbn
pod "vote-7d884dd585-86dbn" deleted
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications# kubectl get po -o wide
NAME                      READY   STATUS    RESTARTS   AGE   IP          NODE             NOMINATED NODE   READINESS GATES
db-58cc845644-4c7rk       1/1     Running   0          28h   10.44.0.1   ip-172-31-1-38   <none>           <none>
mongodb                   1/1     Running   0          26h   10.44.0.6   ip-172-31-1-38   <none>           <none>
redis-6878558678-5pp4v    1/1     Running   0          28h   10.44.0.4   ip-172-31-1-38   <none>           <none>
result-86bc6f7b5d-nr7kc   1/1     Running   0          28h   10.44.0.3   ip-172-31-1-38   <none>           <none>
vote-7d884dd585-7hbl6     1/1     Running   0          10s   10.44.0.7   ip-172-31-1-38   <none>           <none>
worker-6fc5d5b668-6xstl   1/1     Running   0          28h   10.44.0.2   ip-172-31-1-38   <none>           <none>
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications#


2nd ► WORKER pod  ► Observe what happens both in frontEnd & in Unix

In the Unix worker pod has been deleted and it has recreated immediately with different container ID and Ip also changed.

In front end we didnot observe any behaviour change in vote and result page.

root@ip-172-31-1-38:~/example-voting-app/k8s-specifications# kubectl delete po worker-6fc5d5b668-6xstl
pod "worker-6fc5d5b668-6xstl" deleted
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications# kubectl get po -o wide
NAME                      READY   STATUS    RESTARTS   AGE     IP          NODE             NOMINATED NODE   READINESS GATES
db-58cc845644-4c7rk       1/1     Running   0          28h     10.44.0.1   ip-172-31-1-38   <none>           <none>
mongodb                   1/1     Running   0          26h     10.44.0.6   ip-172-31-1-38   <none>           <none>
redis-6878558678-5pp4v    1/1     Running   0          28h     10.44.0.4   ip-172-31-1-38   <none>           <none>
result-86bc6f7b5d-nr7kc   1/1     Running   0          28h     10.44.0.3   ip-172-31-1-38   <none>           <none>
vote-7d884dd585-7hbl6     1/1     Running   0          3m50s   10.44.0.7   ip-172-31-1-38   <none>           <none>
worker-6fc5d5b668-knpzt   1/1     Running   0          7s      10.44.0.5   ip-172-31-1-38   <none>           <none>
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications#


3rd ► DB pod ► Observe what happens both in frontEnd & in Unix
In the Unix worker pod has been deleted and it has recreated immediately with different container ID and Ip also changed.
And one more observation is that result pod and worker pod got restarted.

In the result page the old data got flushed due to DB deletion. Later its started working as expected.

root@ip-172-31-1-38:~/example-voting-app/k8s-specifications# kubectl delete po db-58cc845644-4c7rk
pod "db-58cc845644-4c7rk" deleted
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications# kubectl get po -o wide
NAME                      READY   STATUS    RESTARTS     AGE     IP          NODE             NOMINATED NODE   READINESS GATES
db-58cc845644-cpcv9       1/1     Running   0            39s     10.44.0.2   ip-172-31-1-38   <none>           <none>
mongodb                   1/1     Running   0            26h     10.44.0.6   ip-172-31-1-38   <none>           <none>
redis-6878558678-5pp4v    1/1     Running   0            28h     10.44.0.4   ip-172-31-1-38   <none>           <none>
result-86bc6f7b5d-nr7kc   1/1     Running   1 (8s ago)   28h     10.44.0.3   ip-172-31-1-38   <none>           <none>
vote-7d884dd585-7hbl6     1/1     Running   0            8m14s   10.44.0.7   ip-172-31-1-38   <none>           <none>
worker-6fc5d5b668-knpzt   1/1     Running   1 (8s ago)   4m31s   10.44.0.5   ip-172-31-1-38   <none>           <none>
root@ip-172-31-1-38:~/example-voting-app/k8s-specifications#
