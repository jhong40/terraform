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
terraform show       # show state
terraform providers
terraform output     #
terraform refresh    # depreciate
terraform graph

terraform state list  # list of resource in state: aws_iam_role.alb_role
terraform state show aws_iam_role.alb_role  # show resource details

terrafrom taint aws_instance.webserver   # make it taited, it will be replaced when plan / apply
terraform untaint aws_instance.webserver

terrafrom import 

export TF_LOG=TRACE
export TF_LOG_PATH=/tmp/terraform.log



lifecycle -> ignore_changes: tags, all


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



![image](https://github.com/jhong40/terraform/assets/13383120/fea39cee-6bee-4a5b-b0fb-5f7e8dd5e08b)
![image](https://github.com/jhong40/terraform/assets/13383120/51f78cde-9510-4218-a38a-4f2f1e3d52fd)
![image](https://github.com/jhong40/terraform/assets/13383120/2b098bb1-1e18-4ca1-95f6-4824b7c1ecc7)

![image](https://github.com/user-attachments/assets/0074dbd2-4aef-485f-8070-160428a58f47)


