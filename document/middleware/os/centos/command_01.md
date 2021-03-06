## Command

목차

1. [User 관리](#user-관리)
1. [Password 관리](#password-관리)
1. [Group 관리](#group-관리)
1. [파일 관리](#파일-관리)
1. [네트워크 관련](#네트워크-관련)
1. [Disk 관리](#disk-관리)

* * *

### User 관리

#### * User 정보 확인

```bash
]$ id           # 현재 User의 정보 확인
]$ id admin     # 'admin' User 정보 출력. 없으면 "No such user" 출력
]$ id -g admin  # 'admin' User의 기본 그룹의 gid 출력
]$ id -G admin  # 'admin' User의 User가 속한 모든 그룹의 gid 출력
]$ id -u admin  # 'admin' User의 uid 출력
```
> -n : 위 옵션과 함께 사용해 숫자 대신 이름 출력

#### * 전체 User 목록 확인

```bash
]$ cat /etc/passwd
root :  x  :  0  :  0  :  root  :  /root  :  /bin/bash
(1)    (2)   (3)   (4)    (5)       (6)         (7)
```

> (1) Login Name: 사용자 계정  
(2) Password: 사용자 암호(/etc/shadow 파일에 저장됨)  
(3) User ID: 사용자 ID(root의 경우 0)  
(4) User Group ID: 사용자가 속한 그룹 ID(root 그룹의 경우 0)  
(5) Comments: 사용자 Comment 정보  
(6) Home Directory: 사용자의 홈 디렉토리  
(7) Shell: 사용자가 기본으로 사용하는 쉘 종류  

#### * 현재 로그인 User 확인

```bash
]$ users
```

#### * User 생성

```bash
]$ useradd 유저명            # User 생성. CentOS 에서는 자동으로 홈 디렉토리와 쉘 환경이 설정됨
]$ useradd 유저명 -G 그룹명  # 그룹을 지정하여 생성
]$ useradd 유저명 -u 유저ID  # UID(User ID)을 지정하여 생성
```
#### * User 삭제

```bash
]$ userdel 유저명     # 지정한 User 삭제
]$ userdel -r 유저명  # 지정한 User 삭제(홈 디렉토리도 함께 삭제)
```

#### * sudo 권한 부여

root 계정으로 아래 진행
```bash
]$ cat /etc/sudoers   # sudo 설정 파일 내용 확인
  ~~ 중략 ~~
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
  ~~ 중략 ~~
]$ chmod 755 /etc/sudoers # 파일 수정을 위해 퍼미션 변경(기존:440)
]$ vi /etc/sudoers        # 파일에 [userid] ALL=(ALL:ALL) ALL 와 같이 사용자 계정 추가
$ chmod 440 /etc/sudoers  # 파일 수정을 위해 퍼미션 원복
```


* * *

### Password 관리

#### * Password 확인

```bash
]$ cat /etc/shadow
Root : $1$Fz4q1GjE$G/EskZPyPdMo9.cNhRKSY.:14806: 0 : 99999 : 7 :      :      :
(1)                (2)                     (3)  (4)   (5)   (6)   (7)    (8)    (9)
```

> (1) Login Name: 사용자 계정  
(2) Encrypted: 패스워드 암호 값  
(3) Last Changed: 1970년부터 1월 1일부터 패스워드가 수정된 날짜의 일수  
(4) Minimum: 패스워드 변경 전 최소 사용 일수  
(5) Maximum: 패스워드 변경 전 최대 사용 일수  
(6) Warn: 패스워드 사용 만기일 전에 경고 메시지를 제공하는 일수  
(7) Inactive: 로그인 접속 차단 일수  
(8) Expire: 로그인 사용 금지 일수(월/일/연도)  
(9) Reserved: 사용 안함  

#### * Password 변경

```bash
]$ passwd         # 현재 User Password 변경
]$ passwd admin   # "admin" 인 User Password 변경
```

* * *

### Group 관리

```bash
]$ cat /etc/group          # User Group 확인
]$ groupadd admin          # "admin" Group을 생성(Group ID의 마지막 다음 번호로 ID가 생성됨, 500이 최소값)
]$ groupadd -g 1000 admin  # ID가 "1000"인 "admin" Group을 생성(ID 를 지정하여 생성)
]$ groupadd -r sysadmin    # "sysadm" Group을 시스템용 그룹으로 생성(GID 499 이하)
                           #  (-r 옵션 사용 시 0 번 부터 499 까지에서 미 할당 GID 중 가장 높은 번호를 할당)
]$ gpasswd -a aaa ggg      # "aaa" 사용자를 "ggg" 그룹에 설정
```

* * *

### 파일 관리

#### * 파일 검색

```bash
]$ find . -name "*.war"           # 현재 디렉토리 하위에서 이름이 '*.war'인 파일을 찾아 출력
]$ find . -name "*.war" > out.txt # 현재 디렉토리 하위에서 이름이 '*.war'인 파일 검색 출력 결과를 out.txt에 저장
]$ find / -name "*.war"           # 전체 디렉토리에서 이름이 '*.war'인 파일을 찾아 출력
]$ find / -name "*.war" -ls       # 전체 디렉토리에서 이름이 '*.war'인 파일을 찾아 ls 형식으로 출력
]$ find . -name "std*" -type d    # 현재 디렉토리 하위에서 이름이 'std*'인 디렉토리를 찾아 출력
```
#### * 파일 압축/해제

```bash
]$ tar -cvzf aa.tar.gz dirA dirB  # 디렉토리 A와 B를 aa.tar.gz로 압축
]$ tar -xvf abc.tar               # tar 압축 해제
]$ tar -xzvf abc.tar.gz           # tar.gz 압축 해제
]$ unzip -xzvf abc.tar.gz         # zip 압축 해제
```

#### * 파일 이동

```bash
]$ mv ./* ../     # 현재 디렉토리의 모든 디렉토리 및 파일을 상위로 이동
```

* * *

### 네트워크 관련

#### * SFTP 연결

```bash
]$ sftp root@123.456.7.8      # sftp로 다른 Server에 접속 
ls                            # 상대 Server에서 수행하는 ls 명령어
get aaa.tar.gz                # 상대 Server의  aaa.tar.gz 파일을 가져와서 저장
lls                           # 내 Server에서 수행하는 ls 명령어
exit                          # sftp 종료
```

#### * nslookup 으로 Name Server 관련 조회
```bash
]$ nslookup google.com
Server:         210.220.163.82
Address:        210.220.163.82#53

Non-authoritative answer:
Name:   google.com
Address: 211.49.146.118
Name:   google.com
Address: 211.49.146.119
  ~~ 중략 ~~
```
> 설명: TODO

* * *

### Disk 관리

#### * 남은 Disk 용량 확인하기: df

```bash
]$ df -h -T   
Filesystem          Type      Size  Used Avail Use% Mounted on
/dev/mapper/cl-root xfs        31G  7.7G   23G  26% /
devtmpfs            devtmpfs  905M     0  905M   0% /dev
tmpfs               tmpfs     920M  100K  920M   1% /dev/shm
tmpfs               tmpfs     920M  8.8M  912M   1% /run
tmpfs               tmpfs     920M     0  920M   0% /sys/fs/cgroup
/dev/sda1           xfs      1014M  172M  843M  17% /boot
tmpfs               tmpfs     184M   12K  184M   1% /run/user/1000
```
> -h, --human-readable : print sizes in human readable format (e.g., 1K 234M 2G)  
> -T, --print-type : print file system type


#### * Disk 사용량 확인하기: du(disk usage)
```bash
]$ du      # 현재 경로 하위의 모든 디렉토리의 사용량 확인(Default,KB)
]$ du -h   # 현재 경로 하위의 모든 디렉토리의 사용량 확인(보기좋은 단위로 표시)
]$ du -sh ./   # 해당 디렉토리 사용량 총합 확인
]$ du -sh ./*  # 해당 디렉토리의 바로 하위 디렉토리들의 사용량만 확인
```
> -s, --summarize : display only a total for each argument

* * *

### [References]
1. <http://webdir.tistory.com/134>
1. <http://as-one.tistory.com/entry/CentOS-사용자-관리>
1. <http://welchsy.tistory.com/244](http://welchsy.tistory.com/244>
