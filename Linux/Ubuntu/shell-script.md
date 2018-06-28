# Linux shell script (셸 스크립트)

## shell script
- 셸 스크립트 : 셸 작업을 자동화하도록 만든 각본
  - 단순한 텍스트 파일로 내부는 명령어 실행 절차 기술
  - 스크립트를 만들어두면 그 후로는 재사용 가능
- `#!/bin/bash` : 셔뱅(shebang), 스크립트를 실행하는 프로그램(인터프리터)을 지정, Hash-Bang이라고도 한다.
  - 뒤에 프로그램 전체 경로를 적으면 스크립트를 실행하는 프로그램을 셸이 자동으로 전환해준다.
- 실행 권한 설정 : `chmod +x`
- `if [$? != 0]; then exit; fi` : 이전 명령어가 이상 종료하면 그 자리에서 실행을 중단

## 셸 변수
- 일괄 치환 : vim -> esc -> `:%s / 원문 / 수정문 /`
  - / <- 명령어 각 부분을 구분하는 문자 (/ 이외에도 사용 가능)
- 변수 정의 : `변수명 = 변수값`, ex) `log=/var/log/apache2/access.log`
- `$변수명` 또는 `${변수명}`으로 사용 ex) `less $log, vim $log, cd $workdir ...`
- eval : 문자열을 eval 명령어에 넘기면 명령어열로 실행 가능.
- 유지 보수하기 쉽도록 변수명은 변수에 저장될 내용을 잘 설명하는 이름 사용.

## 환경변수
- 환경 변수를 사용하면 셸 스크립트 실행 시 값이 변하므로 환경에 맞는 처리가 가능해짐
- env : 지금 환경에서 어떤 환경 변수를 쓸 수 있는지 목록 확인.
- `$(명령어열)` : 명령어열 실행 결과를 문자열로 적은 것과 같아짐(명령어 치환)
  - mv result.txt result-`$(date +%Y-%m-%d)`.txt
- 명령어 치환에서 파이프라인이나 변수 등도 사용 가능
  - `running_process=${px x | grep "backup.sh" | grep -v grep}`
    - 프로세스 목록 출력, 찾을 프로세스 backup.sh, grep 자체는 제외
  - `parent=$(dirname $path)`
  - `grandparent=$(dirname $parent)` <- 부모의 부모 디렉토리 경로

## cut
- 로그 파일에서 필요한 줄만 뽑고 싶은 경우
- cut : 파이프로 넘어온 내용의각 줄마다 필요한 부분만 잘라내서 돌려준다.
- `--delimiter "구분자"` 또는 `-d "구분자"`
  - -d 옵션을 사용하지 않으면 default 'tab'이 구분자가 된다.
- `--fields 추출할 위치` 또는 `-f 추출할 위치`
- 넘어온 내용의 각각의 줄을 -d 옵션으로 지정한 구분자로 분할하고 -f 옵션으로 지정한 위치 부분을 추출한 결과를 모아서 출력한다.
- `cut -d " " -f 7` : 스페이스를 구분자로 지정, 7번째 부분만 모아서..
- `echo "$PWD" | cut -d "/" -f 2`

## sort uniq
- sort : 입력된 내용을 알파벳 순서(오름차순)로 재정렬
  - `cat input.txt | sort`
  - `--reverse, -r` : 내림차순 정렬
  - `-t` : 구분자 지정
  - `-k` : 열 지정
  - `--number, -n` : 숫자로 해석해서 값의 크기로 재정렬
  - `-b` : 스페이스는 무시
- uniq : 같은 내용의 **이어진** 중복 제거
  - `cat input.txt | uniq`
  - 미리 sort 해두면 모든 중복을 제대로 제거 가능
  - `--count, -c`옵션으로 uniq 실행 결과에 각각 내용이 몇번 등장했는가 출력
- head, tail : -n 옵션으로 출력 줄 수 변경가능(default : 10)
- CSV(Comma Seperated File) : 구분자 콤마를 써서 줄의 내용을 필드 단위로 나눈 텍스트 파일
  - `abc.csv | cut -d "," -f 3` (-f옵션 : 열번호 - 여러 숫자, 열 범위 지정 가능)

## 리다이렉트
- 명령어 실행 결과를 저장
- `>` : 파이프라인으로 넘어가는 것과 같은 내용이 텍스트 파일로 출력된다.
- `>` 하나이면 이미 파일이 있으면 지우고 새로운 파일 만듬
- `>>` 두 번 이어서쓰면 기존 파일에 추가한다.

## 명령줄 인수 (명령줄 지정으로 작업 내용을 바꾸고 싶을 때)
- 인수 : 명령어 라인 인수의 줄임말, 명령어에 대해 추가 지시를 내림
  - `cat /var/log/apache2/access.log`
  - 위의 명령에서 /var/log/apache2/access.log에 해당
- cp명령의 -r같이 **생략 가능한(지정하면 행동이 변하는) 인수** 를 옵션인수, 줄여서 옵션이라고 부른다.

### 셸 스크립트에서 인수를 사용하는 방법
- 셸 스크립트 내부에서는 실행시 지정한 인수 값을 $1 같은 변수로 참조할 수 있다.
```bash
지정한 순서대로 값을 참조함
$script.sh filename1 filename2
#!/bin/bash
source=$1
target=$2
...
```
- 인수를 이름으로 참조(추상화)
```bash
옵션 이름으로 값을 참조함
$script.sh -s filename1 -t filename2
#!/bin/bash
while getpots s:t: OPT
do
  case $OPT in
    s) source="$OPTARG" ;;
    t) target="$OPTARG" ;;
  esac
done
```
- `s:t:` : 옵션 이름으로 사용하는 알파벳, 한 글자 뒤에 :을 붙임
- `s)` : source라는 변수로 -s 값을 참조할 수 있도록 함

## Links
- [만화로 배우는 리눅스 시스템 관리 1](http://book.naver.com/bookdb/book_detail.nhn?bid=10995037)
