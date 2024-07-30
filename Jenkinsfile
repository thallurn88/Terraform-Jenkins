pipeline{

parameters{
    booleanParam(name: autoapprove, defaultValue: false, description:"apply the changes once the plan completed?")
}

environment{

    secret_Key   = crendentials("secret_key_ID")
    access_Key   = crendentials("access_key_ID")
}

agent any


stages{

 stage("Checkout the terraform config"){
   steps{
    git https://github.com/thallurn88/Terraform-Jenkins.git
   }
  }




stage("Plan"){
   steps{
    sh 'terrform init'
    sh 'terrform plan -out tfplan'
    sh 'terrform show -no-colour tfplan > tfplan.txt'
   }
}



stage("Approve"){
    when{
        not{
            equals expected: true, actual: params.autoapprove
        }
    }
   steps{
    scripts{
        def plan = readFile tfplan.txt
        input message: "do you want apply the changes?"
        parameters: [text(name: plan , defaultValue: plan, decription: "Please review the plan")]
    }
   }
}




stage("Apply"){
   steps{

    sh "terraform apply -input=false tfplan"

   }
}




}
}
