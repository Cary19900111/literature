# pod重启原因查询
* k logs -p pod_name -n ns
* k describe pod pod_name -n ns
* 查看节点系统 /var/log/messages