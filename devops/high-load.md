# Step 1
Figure out if it's actually an issue

- Load is the amount of processes waiting for cpu time, so for things like SMTP servers, high load is normal
- Check response times on different applications, if its slow, it might be the loads fault
- Compare load on "slow" server to other server in cluster

# Step 2
Figure out where bottleneck is, there are 3 possibilities

### I/O is slow

- The I/O might be an issue, tools to check this include: iotop, iostat and general monitoring (grafana, kibana, prometheus)
- To check status on different processes, open top -H (-H shows threads too), press f, select S via arrow keys (S is filtered by process state), press q and then press shift + R to reverse the sorting (State D will be on top, which are usually caused by I/O issues)

### Ram is overloaded

- Check ram usage via free -m, top, htop and general monitoring tools
- If this is the culprit, locate possible reasons for high ram usage(zombies, unkilled orphans, bad code)

### CPU

- Check cpu usage via top, htop and general tools
- If this is the culprit, locate possible reasons for high cpu usage(infinite loops, unreasonable amount of one process, bad code)