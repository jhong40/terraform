# terraform

https://github.com/angelo-malatacca/Terraform-backend

deep dive
https://github.com/ned1313/Deep-Dive-Terraform

```
terraform plan -no-color > tfout.txt    
```

```
terraform init
terraform plan
terraform apply
terraform validate   # validate the syntax of all tf files
terraform show       # show all the resources  (terraform state show - show one resource)
terraform providers
terraform output     #
terraform refresh    # depreciate
terraform graph

terraform state list                        # list of resource in state: aws_iam_role.alb_role
terraform state show aws_iam_role.alb_role  # show resource details

terrafrom taint aws_instance.webserver   # make it taited, it will be replaced when plan / apply
terraform untaint aws_instance.webserver

terrafrom import 

export TF_LOG=TRACE
export TF_LOG_PATH=/tmp/terraform.log

~> 1.2.3  # right most number >=3  (no change for 1.2): 1.2.3, 1.2.4 (no: 1.3.x 2.x.x)
 


lifecycle -> ignore_changes: tags, all
sensitive: true   # cli => (sensitive value) output => <sensitive> 

resource "aws_instance" "web" {
  ami=
  instance=
  provisioner "local-exec" {        # run this locally where terrafrom is running
    command = "echo ${self.private_ip} >/tmp/ips.txt"
  }
}

resource "aws_instance" "web" {

  connection {
    type     = "ssh"
    user     = "root"
    password = var.root_password
    host     = self.public_ip
  }

  provisioner "remote-exec" {      # run on the remote machine after ssh into it
    inline = [
      "puppet apply",
      "consul join ${aws_instance.web.private_ip}",
    ]
  }
}

```

```
terraform init -backend-conf="bucket=myterraform_backend_bucket"
terraform init -backend-conf="backend-settings.txt"  # kv pair: bucket=myterraform_backend_bucket
```
```
TF_IN_AUTOMATION=TRUE
TF_LOG="INFO"
TF_LOG_PROVIDER="ERROR"
TF_LOG_PATH="PATH"
TF_INPUT=FALSE
TF_VAR_myname="VALUE"
TF_CLI_ARGS="COMMAND_flags"  # -no-color

```

### Workspace
```
locals {
  instance_type={
    default = "t2.nano"
    dev = "t2.micro"
    prod = "m5.large"
  }
}

resource "aws_instance" "myec2" {
  ami   = "ami_aaabbb1234"
  instance_type = local.instance_type[terraform.workspace]
}

drawback
- Share State data
- Testing and Promotion challeng (Terrform doesn't know branch or dir)
- Variable Value management
```
### Separate Directory and Child Modules
```
 Environment
   dev
     main.tf
     dev.tfvars
     backend.tf
   prod
     main.tf
  modules
    network
    security
    ec2
```


![image](https://github.com/user-attachments/assets/93838552-cf4b-4f79-82c4-c5cba32b11dd)


![image](https://github.com/user-attachments/assets/c8f94bcd-1dc7-4d2a-9e0c-d04ededb1f80)
![image](https://github.com/user-attachments/assets/85f430f9-5fa9-4a64-9d46-4907910289e6)


![image](https://github.com/jhong40/terraform/assets/13383120/2b098bb1-1e18-4ca1-95f6-4824b7c1ecc7)

![image](https://github.com/user-attachments/assets/56257db2-855d-4229-ac8c-ef4166a53e1c)
![image](https://github.com/user-attachments/assets/fe23a9fb-944c-42c1-a8c7-f0c263035ac0)
![image](https://github.com/user-attachments/assets/5a0b6be4-d04c-47fa-9b2d-0d2a1a75d2e9)







