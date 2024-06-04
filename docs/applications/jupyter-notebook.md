
This guide will show you how to start a Jupyter notebook on AI-LAB and access it in your browser.

To begin, log in to AI-LAB and run the following command:

```console
srun --pty /bin/bash
```

This command allocates a compute node for you. The `--pty /bin/bash` part ensures that a pseudo-terminal is created and a bash shell is started on the compute node.

After the node is allocated, you will see the name of the compute node in the terminal, which will look something like this:

```console
<email>@ailab-l4-01:~$
```

In this example, the compute node allocated is `ailab-l4-01`.

Next, you need to find an available port to host the Jupyter notebook on. Enter the following command:

```
python3 -c "import socket; s = socket.socket(); s.bind(('localhost', 0)); print(s.getsockname()[1]); s.close()"
```

This will provide you with an available port number, e.g `37429`.

You can now start a Jupyter notebook. Jupyter is included in the PyTorch and TensorFlow containers found in the container directory on AI-LAB. First, copy the PyTorch container image from the container directory on AI-LAB to your user directory (it may take a few minutes):

```console
cp /ceph/container/pytorch_24.03-py3.sif .
```

Then, run the following command to start a Jupyter notebook:

```
srun singularity exec tensorflow_24.03-tf2-py3.sif jupyter notebook --port <port number>
```

Replace `<port number>` with the one you got.

Next, you need to set up port forwarding so you can access the notebook from your local computer. 

First, open up a new terminal window. Use the following command, replacing `<email>` with your AAU email and `<port number>` with the port number you received before:

```console
ssh -L <port number>:localhost:<port number> <email>@ai-fe02.srv.aau.dk
```

This command sets up a port tunnel between AI-LAB and your local machine, allowing you to access the notebook via your local browser.

Open your browser and go to [localhost:[port number]](#) to access the notebook. The token required to log in will be displayed in the terminal output.

```console
[I 08:30:06.090 NotebookApp] http://hostname:8888/?token=8db29582846a0a31dfbd5f539f8a5097252011919832d0bb
```

You can run an interactive session for a maximum of 12 hours. For more details, see the [guidelines](/guidelines/#4-jobs-can-run-no-longer-than-12-hours). It is recommended to stop the job as soon as you are done working interactively. Closing the terminal window where you ran `srun --pty /bin/bash` will stop the job.