Go to your Pipeline Script:
```
def greet(name) {
  echo "Hello, ${name}"
}

def multiplication(number) {
  return number * 2
}

pipeline{
  agent any
  stages{
    stage('Calling functions'){
      steps{
        script{
          greet('Jenkins')
          def result = multiplication(5)
          echo "Output is ${result}"
        }
      }
    }
  }
}
```
