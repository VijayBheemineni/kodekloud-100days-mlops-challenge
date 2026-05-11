# Fix Jupyter configuration file
For the running Jupyter server must meet the following requirements:

it listens on port 8888;
it binds on 0.0.0.0 (the lab proxy cannot reach a server that is only bound on 127.0.0.1);
the notebook root directory is /root/notebooks/, and that directory exists on disk.

```
# Create the /root/notebooks/ directory
mkdir -p /root/notebooks/

# Jupyter configuration file for the xFusionCorp Industries data science team

# --- xFusionCorp team overrides (review before starting the server) ---
# Update the jupyter config file with right settings
c.ServerApp.token = ''
c.ServerApp.password = ''
c.ServerApp.disable_check_xsrf = True
c.ServerApp.notebook_dir = '/root/notebooks/'
c.ServerApp.port = 8888
c.ServerApp.ip = '0.0.0.0'

# Start the jupyter server
source /root/code/ml-env/bin/activate
jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root --no-browser &
```