Building an AWS VPC (Virtual Private Cloud) using Terraform involves several steps. Below is a step-by-step guide to help you create an AWS VPC using Terraform:

1. **Install Terraform**: If you haven't already, download and install Terraform on your local machine. You can find installation instructions on the official Terraform website.

2. **Set Up AWS Credentials**: Ensure you have AWS credentials set up on your machine. You can either set them as environment variables or use AWS CLI to configure them.

3. **Create Terraform Configuration Files**:

   Create a directory for your Terraform configuration and navigate into it.
   Inside this directory, create a file named `main.tf` where you'll define your Terraform configuration.

4. **Define AWS Provider**:

   In your `main.tf` file, add the following code to define the AWS provider:

   ```hcl
   provider "aws" {
     region = "your-region"
   }
   ```

   Replace `"your-region"` with your desired AWS region.

5. **Create VPC Configuration**:

   Add the VPC configuration to your `main.tf` file:

   ```hcl
   resource "aws_vpc" "my_vpc" {
     cidr_block = "10.0.0.0/16"
     tags = {
       Name = "my-vpc"
     }
   }
   ```

   This creates a VPC with CIDR block `10.0.0.0/16` and assigns the tag "Name" with the value "my-vpc".

6. **Create Subnet Configuration**:

   Add the subnet configuration to your `main.tf` file:

   ```hcl
   resource "aws_subnet" "my_subnet" {
     vpc_id            = aws_vpc.my_vpc.id
     cidr_block        = "10.0.1.0/24"
     availability_zone = "your-az"
     tags = {
       Name = "my-subnet"
     }
   }
   ```

   Replace `"your-az"` with your desired availability zone.

7. **Define Route Table**:

   ```hcl
   resource "aws_route_table" "my_route_table" {
     vpc_id = aws_vpc.my_vpc.id

     route {
       cidr_block = "0.0.0.0/0"
       gateway_id = aws_internet_gateway.my_igw.id
     }

     tags = {
       Name = "my-route-table"
     }
   }
   ```

8. **Create Internet Gateway**:

   ```hcl
   resource "aws_internet_gateway" "my_igw" {
     vpc_id = aws_vpc.my_vpc.id

     tags = {
       Name = "my-igw"
     }
   }
   ```

9. **Associate Route Table with Subnet**:

   ```hcl
   resource "aws_route_table_association" "subnet_association" {
     subnet_id      = aws_subnet.my_subnet.id
     route_table_id = aws_route_table.my_route_table.id
   }
   ```

10. **Initialize Terraform**:

    Run `terraform init` in your terminal inside the directory where your Terraform configuration files are located.

11. **Review and Apply Configuration**:

    Run `terraform plan` to review the changes that Terraform will make. If everything looks good, apply the changes by running `terraform apply`.

12. **Verify**:

    Log in to your AWS Management Console and navigate to the VPC dashboard to verify that your VPC has been created successfully.

13. **Destroy**:
    Run `terraform destroy` in your terminal inside the directory where your Terraform configuration files are located.
    The terraform destroy command is a convenient way to destroy all remote objects managed by a particular Terraform configuration.

