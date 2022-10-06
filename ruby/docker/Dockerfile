# syntax=docker/dockerfile:1
FROM ruby:3.0

# copy in files
RUN mkdir /src
COPY /ruby/* /src/
ADD ../../.github/linters/.ruby-lint.yml /src/.ruby-lint.yml

# resolve dependencies
WORKDIR /src
RUN bundle config --delete frozen
# RUN bundle install

# configure AWS credentials
RUN mkdir .aws
COPY aws_configure.sh /
RUN chmod 755 /aws_configure.sh

# ENTRYPOINT ["/aws_configure.sh"]
CMD ["bash"]

# STEPS FOR BUILDING & PUSHING IMAGES
# 1. fetch security tokens
#
# 2. aws ecr get-login-password --region us-east-1 | docker login \
#   --username AWS \
#   --password-stdin 260778392212.dkr.ecr.us-east-1.amazonaws.com
#
# 3. docker build . -t test-harness -f ruby/ruby.Dockerfile
#
# 4. docker tag test-harness:latest 260778392212.dkr.ecr.us-east-1.amazonaws.com/test-harness:latest
#
# 5. docker push 260778392212.dkr.ecr.us-east-1.amazonaws.com/test-harness:latest

# IDENTITY PROVIDER: token.actions.githubusercontent.com
# ARN: arn:aws:iam::260778392212:oidc-provider/token.actions.githubusercontent.com

# IAM ROLE:
# https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/stackinfo?stackId=arn%3Aaws%3Acloudformation%3Aus-east-1%3A260778392212%3Astack%2Ftest%2F183f05a0-43ad-11ed-a564-0e8a42e04a6b

# CREATE NEW POLICY
#{
#  "Version": "2012-10-17",
#  "Statement": [
#    {
#      "Effect": "Allow",
#      "Principal": {
#        "Federated": "arn:aws:iam::260778392212:oidc-provider/token.actions.githubusercontent.com"
#      },
#      "Action": "sts:AssumeRoleWithWebIdentity",
#      "Condition": {
#        "StringEquals": {
#          "token.actions.githubusercontent.com:aud": "sts.amazonaws.com",
#          "token.actions.githubusercontent.com:sub": "repo:ford-at-aws/aws-doc-sdk-examples:ref:refs/heads/main"
#        }
#      }
#    }
#  ]
#}