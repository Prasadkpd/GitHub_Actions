# GitHub Actions

A brief discription about github actions

#### First Github Action
    name: Hello
    
    on: 	#Event
      push: 	#Condition(only run when push to a master)
        branches: master
      pull_request: #Condition(only run when PR to a master)
        branches: master
    jobs:	#Jobs
      job1:
        name: First job
        runs-on: ubuntu-latest
        steps:
          - name: Step one
            uses: actions/checkout@v2	#Action
          - name: Step two
            run: |
		echo "Hello"
		echo "World"
      job2:
        name: Second job
        runs-on: windows-latest
        needs: job1 #Dependencies(Have to run job1 first)
        steps:
          - name: Step one
            uses: actions/checkout@v2
          - name: Step two
            run: "Get-ChildItem Env: | Sort-Object Name"
    
#### Second Github Action

- Push Trigger
- Multiple runners
- Job Dependencies

    name: Second Action
    
    on: 
      push:
        branches: master
      pull_request:
        branches: master
    jobs:
      job1:
        name: Ubuntu
        runs-on: ubuntu-latest
        steps:
          - run : date
      job2:
        name: Windows
        runs-on: windows-latest
        steps:
          - run : date
      job3:
        name: MacOs
        runs-on: macos-latest
        steps:
          - run : date
      job4:
        name: Depends
        needs: [job1,job2,job3]
        runs-on: ubuntu-latest
        steps:
          - run : date

###### Passign Argument to and Action

https://github.com/automate6500/passing-arguments-to-an-action/blob/master/.github/workflows/build-tomcat.yml
