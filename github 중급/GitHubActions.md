# GitHubActions 핵심 개념
## workflow
- GitHubActions에서 자동화된 전체 프로세스를 말한다.
- 하나 이상의 Job으로 구성됨.
- Event에 의해 예약되거나 트리거될 수 있는 자동화된 절차이다.
- GitHubRepository의  .github/workflows 폴더 아래에 Workflow YAML 파일을 저장할 수 있다.
- 이 파일로 정의한 자동화 동작을 Github에게 전달하면 GitHubActions는 해당 파일을 기반으로 그대로 실행시킨다.
- ![[Pasted image 20240422233349.png]]
## Event
- workflow를 실행하는 특정 Action, 트리거, 규칙을 의미한다.
- 누군가가 repository에 코드를 push 하거나 PR 했을 때 Github에서 Workflow가 실행될 수 있다.
## Job
- 여러 step으로 구성되고, 단일 가상 환경에서 실행.
- 다른 Job에 의존 관계를 가질 수도 있고, 독립적으로 병렬로 실행될 수도 있다.
- GitHubActions에서 Job은 작업 단위를 의미한다. 하나의 작업은 주어진 작업을 완료하기 위해 수해오디는 단일 프로세스.
- Workflow 파일에 정의된 여러 작업이 순차적 또는 병렬로 실행될 수 있다.
## step
- Job 안에서 순차적으로 실행되는 프로세스 단위이다. Step에서는 명령을 내리거나, Action을 실행할 수 있다.
- jobs.build.steps: 로 액션의 실제 수행 내용을 쓸 수 있고, 단계별로 name과 수행 동작을 적어준다.
## Action
- 재사용 가능한 workflow 구성요소. GitHub Marketplace에서 제공되며, 커뮤니티나 개인이 만든 수많은 액션이 있다.
- 이러한 액션들은 Github Actions 워크플로우를 효율적으로 구성하고 자동화 하는데 도움이 된다.
## Runner
- Gtihub Action Runner 어플리케이션이 설치된 머신. Workflow가 실행될 인스턴스를 Runner라고 한다.