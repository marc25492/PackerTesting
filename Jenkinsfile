#!groovy

node {
  environment {
      AWS_ACCESS_KEY = credentials('AWS_ACCESS_CREDENTIALS')
      AWS_SECRET_KEY = credentials('AWS_SECRET_KEY')
  }  
    
  def err = null
  currentBuild.result = "SUCCESS"

  try {
    stage 'Checkout'
      checkout scm

    stage 'Validate'
      def packer_file = 'packer.json'
      print "Running packer validate on : ${packer_file}"
      sh "/usr/local/packer validate ${packer_file}"

    stage 'Build'
      sh "/usr/local/packer build -var 'aws_access_key=$AWS_ACCESS_KEY'  -var 'aws_secret_key=$AWS_SECRET_KEY' ${packer_file}"

    stage 'Test'
      print "Testing goes here."
  }

  catch (caughtError) {
    err = caughtError
    currentBuild.result = "FAILURE"
  }

  finally {
    /* Must re-throw exception to propagate error */
    if (err) {
      throw err
    }
  }
}
