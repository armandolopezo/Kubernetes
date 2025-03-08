# This INGRESS.YAML is not working . I need INGRESS.YAML created with GITHUB COPILOT
# using "ANNOTATIONS:" instead "HOST:"
#
# THIS WORK:  
#   annotations:
#    external-dns.alpha.kubernetes.io/hostname: contoso-website.com
#
# THIS fails:
#  rules:
#  - host: contoso-website.com
#