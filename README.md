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

#### Passign Argument to and Action

https://github.com/automate6500/passing-arguments-to-an-action/blob/master/.github/workflows/build-tomcat.yml

#### Using Enviroment Variable

    name: Environment Variables
    env:
      WORKSPACE_ENVIRONMENT_VARIABLE: 'custom workspace environment variable'
    on: [push]
    jobs:
      ubuntu:
        env:
          JOB_ENVIRONMENT_VARIABLE: 'custom job environment variable for ubuntu'
        runs-on: ubuntu-latest
        steps:
          - name: Print custom environment variables from ubuntu-latest
            env:
              STEP_ENVIRONMENT_VARIABLE: 'custom step environment variable for bash'
            run: |
              echo "Accessing environment variables in run command"
              echo "$WORKSPACE_ENVIRONMENT_VARIABLE"
              echo "$JOB_ENVIRONMENT_VARIABLE"
              echo "$STEP_ENVIRONMENT_VARIABLE"
              echo "------------------------------------------------------"
              echo "Accessing environment variables using env context"
              echo "${{ env.WORKSPACE_ENVIRONMENT_VARIABLE }}"
              echo "${{ env.JOB_ENVIRONMENT_VARIABLE }}"
              echo "${{ env.STEP_ENVIRONMENT_VARIABLE }} Not defined"
      windows:
        env:
          JOB_ENVIRONMENT_VARIABLE: 'custom job environment variable for windows'
        runs-on: windows-latest
        steps:
          - name: Print custom environment variables from windows-latest
            env:
              STEP_ENVIRONMENT_VARIABLE: 'custom step environment variable for powershell'
            run: |
              echo "Accessing environment variables in run command"
              echo "$Env:WORKSPACE_ENVIRONMENT_VARIABLE"
              echo "$Env:JOB_ENVIRONMENT_VARIABLE"
              echo "$Env:STEP_ENVIRONMENT_VARIABLE"
              echo "------------------------------------------------------"
              echo "Accessing environment variables using env context"
              echo "${{ env.WORKSPACE_ENVIRONMENT_VARIABLE }}"
              echo "${{ env.JOB_ENVIRONMENT_VARIABLE }}"
              echo "${{ env.STEP_ENVIRONMENT_VARIABLE }} Not defined"
   
#### Using Secret   
   
name: secrets

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}
    - name: List S3 Buckets
      run: aws s3api list-buckets
