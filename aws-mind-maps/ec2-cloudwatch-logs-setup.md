📦 EC2 Instance
│
├── 🛠️ Install CloudWatch Agent
│   ├── Command: `sudo yum install -y amazon-cloudwatch-agent`
│   └── Verify: `amazon-cloudwatch-agent-ctl -h`
│
├── 📝 cloudwatch-config.json
│   ├── **Agent Configuration**
│   │   └── `"metrics_collection_interval": 60`
│   │       - Collects metrics every 60 seconds.
│   │       - Logs CloudWatch agent activity to `/var/log/amazon-cloudwatch-agent.log`
│   │
│   ├── **Logs Configuration**
│   │   └── `"logs_collected": { "files": { "collect_list": [...] } }`
│   │
│   └── **Log Files**
│       ├── 📄 `/home/ec2-user/projects/mite/etc/nginx/access.log`
│       │   ├── `"log_group_name": "nginx-logs"`
│       │   ├── `"log_stream_name": "{instance_id}/nginx/access.log"`
│       │   └── `"timestamp_format": "%d/%b/%Y:%H:%M:%S %z"`
│       │
│       ├── 📄 `/home/ec2-user/projects/mite/etc/nginx/error.log`
│       │   ├── `"log_group_name": "nginx-logs"`
│       │   ├── `"log_stream_name": "{instance_id}/nginx/error.log"`
│       │   └── `"timestamp_format": "%d/%b/%Y:%H:%M:%S %z"`
│       │
│       └── 📄 `/home/ec2-user/projects/mite/django.log`
│           ├── `"log_group_name": "django-logs"`
│           ├── `"log_stream_name": "{instance_id}/django/django.log"`
│           └── `"timestamp_format": "%Y-%m-%d %H:%M:%S"`
│
│
├── ⚙️ Configure CloudWatch Agent
│   ├── Copy Config: `sudo cp cloudwatch-config.json /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json`
│   ├── Attach IAM Role:
│   │   - **Policy**: `CloudWatchAgentServerPolicy`
│   │   - **Action**: AWS Console > EC2 > Actions > Security > Modify IAM Role
│   └── Start CloudWatch Agent:
│       ```bash
│       sudo amazon-cloudwatch-agent-ctl \
│           -a fetch-config \
│           -m ec2 \
│           -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json \
│           -s
│       ```
│       - **Explanation**:
│         - `-a fetch-config`: Fetches configuration from the file.
│         - `-m ec2`: Specifies the environment as EC2.
│         - `-c file:...`: Path to the CloudWatch configuration file.
│         - `-s`: Starts the CloudWatch agent.
│
│
├── 🔄 Automate Log Rotation (logrotate.d)
│   └── File: `/etc/logrotate.d/nginx_django`
│       ```plaintext
│       /home/ec2-user/projects/mite/etc/nginx/access.log
│       /home/ec2-user/projects/mite/etc/nginx/error.log
│       /home/ec2-user/projects/mite/django.log {
│           missingok
│           notifempty
│           copytruncate
│           daily
│           rotate 1
│           dateext
│           postrotate
│               /bin/kill -USR1 `cat /var/run/nginx.pid`
│           endscript
│       }
│       ```
│       - **Key Points**:
│         - **`copytruncate`**: Truncates the log file after rotation.
│         - **`daily`**: Rotates logs daily.
│         - **`rotate 1`**: Keeps only 1 previous log.
│         - **`postrotate`**: Sends a signal to Nginx to reopen log files.
│
│
└── ✅ Verify Everything
    ├── **Check Agent Status**:
    │   ```bash
    │   sudo amazon-cloudwatch-agent-ctl -a status
    │   ```
    │
    ├── **View Logs in AWS CloudWatch**:
    │   - AWS Console > CloudWatch > Log Groups.
    │
    └── **Enable Agent on Boot**:
        ```bash
        sudo systemctl enable amazon-cloudwatch-agent
        ```

