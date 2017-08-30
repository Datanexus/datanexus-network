# network
community network release
## configuration
If any _key:value_ pairs don't apply, comment them out or remove them.
### aws
Certain _key:value_ pairs can be configured per project.

These values should remain unchanged.

    cloud: aws
Replace TENANT with the name of the AWS_PROFILE environment variable. 
    
    tenant: TENANT
Replace PROJECT_NAME with the name of the project hosting the instances. 

    project: PROJECT_NAME
Replace DOMAIN with a `production` or `development` (or another value of your choosing).

    domain: DOMAIN
Replace REGION with your desired AWS region such as `us-east-1` or `ap-southeast-2`.

    region: REGION    
Replace ZONE with the desired AWS availability zone.

    zone: ZONE
Replace CIDR_BLOCK and SUBNET with the values in the VPC, e.g., 10.10.0.0/16, 10.10.1.0/24, 10.10.2.0/24. The internal subnet is private and non-routable, the external subnet will outbound routing.
    
    cidr_block: CIDR
    internal_subnet: SUBNET
    external_subnet: SUBNET
(if needed) Replace  PROXY with `yes` or `no` if there is an HTTP proxy involved in getting to the internet.
 
    http_proxy: PROXY
(if needed) Replace USER, PASSWORD, DOMAIN, PORT with the necessary values.
    
    http_proxy_url: http://USER:PASSWORD@DOMAIN:PORT

## deployment
### amazon web services
To deploy on AWS:

    ./deploy aws
    
You can also run the code on an alternate configuration file, such as `aws-demo.yml`:

    AWS_PROFILE=datanexus ./provision-network -e "configuration=aws-demo.yml"

Expected running times is approximately 3 seconds.

## testing
If the `deploy` code ran without errors, that's a pretty good indicator of a successful deployment. However, you can run the following to be sure.

### amazon web services
To test an AWS deployment:

    ./test aws
        
### expected results

This indicates the `datanexus_development` VPC was successfully created:

    ok: [localhost] => {
        "msg": {
            "changed": false,
            "vpcs": [
                {
                    "cidr_block": "10.10.0.0/16",
                    "classic_link_enabled": null,
                    "dhcp_options_id": "dopt-c7e322a0",
                    "id": "vpc-8b755ef2",
                    "instance_tenancy": "default",
                    "is_default": false,
                    "state": "available",
                    "tags": {
                        "Domain": "development",
                        "Name": "datanexus_development",
                        "Tenant": "datanexus"
                    }
                }
            ]
        }
    }

### cleanup

Cleanup:
    
    AWS_PROFILE=datanexus ./cleanup-network -e "configuration=aws-demo.yml"    
