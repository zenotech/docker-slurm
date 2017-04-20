FROM centos:7 AS build-env

RUN yum -y install epel-release

RUN yum -y install gcc perl-devel perl-CPAN wget mysql-devel openssl openssl-devel pam-devel numactl numactl-devel hwloc hwloc-devel lua lua-devel readline-devel rrdtool-devel ncurses-devel man2html libibmad libibumad rpm-build munge-devel

RUN cd /tmp && wget http://www.schedmd.com/download/latest/slurm-17.02.2.tar.bz2 && rpmbuild -ta slurm-17.02.2.tar.bz2

FROM centos:7

RUN yum -y install epel-release

COPY --from=build-env /root/rpmbuild/RPMS/x86_64 /tmp/

RUN cd /tmp && yum -y localinstall slurm-plugins-17.02.2-1.el7.centos.x86_64.rpm slurm-17.02.2-1.el7.centos.x86_64.rpm slurm-munge-17.02.2-1.el7.centos.x86_64.rpm

RUN useradd slurm

RUN touch /var/run/slurmctld.pid && chown slurm /var/run/slurmctld.pid && mkdir /var/spool/slurm && chown slurm /var/spool/slurm

COPY slurm.conf /etc/slurm/slurm.conf

EXPOSE 6818

CMD cp /run/secrets/munge_key /etc/munge/munge.key && chown munge /etc/munge/munge.key && /usr/sbin/munged -f && /usr/sbin/slurmctld -Dvvvvv
