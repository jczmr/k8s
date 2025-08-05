kubectl create deployment mynginx --image=nginx:1.15-alpine

kubectl get deploy,rs,po -l app=mynginx

kubectl scale deploy mynginx --replicas=3


# about rolling upodate and rollback

kubectl rollout history deploy mynginx
kubectl rollout history deploy mynginx --revision=1


kubectl set image deployment mynginx nginx=1.16-alpine


# rollback
kubectl rollout undo deployment mynginx --to-revision=1

