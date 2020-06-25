# cfn-macro-route53-hosted-zone-id

This is an AWS CloudFormation macro to retrieve a Route53 Hosted Zone ID. It can be used when
you are deploying resources into an existing hosted zone that was created outside of CloudFormation,
or for which you don't otherwise have access to the hosted zone id. This macro will retrieve the
hosted zone id from its domain name.

## Creating the Macro

Create the macro by deploying the `template.yml` stack:

    aws [--profile name] cloudformation deploy \
      --capabilities CAPABILITY_IAM \
      --stack-name HostedZoneId \
      --template-file template.yml

Use whatever stack name you wish. The macro name by default will be `HostedZoneId`, but you can change that by adding `--parameter-overrides MacroName=MyName`.

## Using the Macro

Here's an example of provisioning an ACM certificate into a zone with the domain name "mydomain.com":

    Certificate:
      Type: AWS::CertificateManager::Certificate
      Properties:
        DomainName: www.mydomain.com
        DomainValidationOptions:
          - DomainName: www.mydomain.com
            HostedZoneId:
              Fn::Transform:
                Name: HostedZoneId
                Parameters:
                  DomainName: mydomain.com
        ValidationMethod: DNS

> Note that for the `DomainName` parameter of the `Fn::Transform` function, use
> the domain name of the hosted zone, not the domain name of the certificate.
