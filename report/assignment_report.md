## Task 1: System Monitoring Setup

### Objective
Configure basic system monitoring to track CPU, memory, processes, and disk usage for troubleshooting intermittent performance issues and supporting capacity planning.

### Commands Used
```bash
sudo apt update
sudo apt install -y htop

mkdir -p logs/monitoring
TS=$(date +%F_%H-%M-%S)

top -b -n 1 > logs/monitoring/top_$TS.log
ps aux --sort=-%cpu | head -n 15 > logs/monitoring/top_cpu_$TS.log
ps aux --sort=-%mem | head -n 15 > logs/monitoring/top_mem_$TS.log
df -h > logs/monitoring/df_$TS.log
du -sh /home /var /tmp 2>/dev/null > logs/monitoring/du_$TS.log

{
  echo "Timestamp: $(date '+%Y-%m-%d %H:%M:%S %Z')"
  echo "Hostname: $(hostname)"
  echo "Uptime: $(uptime -p)"
  echo
  echo "CPU/Memory quick lines from top:"
  top -b -n 1 | head -n 5
  echo
  echo "Generated log files:"
  ls -1 logs/monitoring/*_"$TS".log
} > logs/monitoring/summary_$TS.log
