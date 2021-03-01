FROM centos:7

# Install openssh and passwd for the necessary future ssh connections
RUN yum update -y && \
    yum install -y openssh-server && \
    yum install -y passwd

# To be able to complete our example in centos, we run:
# 1. Create new user called "remote_user"
# 2. Add password "1234" to "remote_user"
# 3. Create directory ".ssh" in the path "/home/remote_user/"
# 4. Set chmod700 permissions (chmod a+rwx, g-rwx, o-rwx) to ".ssh"
RUN useradd remote_user && \
    echo "1234" | passwd remote_user --stdin && \
    mkdir /home/remote_user/.ssh && \
    chmod 700 /home/remote_user/.ssh

# Copy the locally created public key for the ssh connection
COPY remote_keys_example.pub /home/remote_user/.ssh/authorized_keys

# After adding the public key, we configure the owner and the permissions:
# 1. Change owner of the created public key from "root" to "remote_user"
# 2. Set chmod600 permissions (chmod a+rwx,u-x,g-rwx,o-rwx) to "authorized_keys"
RUN chown -R remote_user:remote_user /home/remote_user && \
    chmod -R 600 /home/remote_user/.ssh/authorized_keys

# RUN /usr/sbin/sshd-keygen
RUN ssh-keygen -A

# Main command to keep running the container with main process
CMD /usr/sbin/sshd -D
