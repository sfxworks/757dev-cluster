

1. Register at https://keycloak.sfxworks.net/auth/realms/757dev/account
2. Install kubelogin https://github.com/int128/kubelogin/blob/master/README.md
3. Make a pull request for a namespace / rbac at {{.Values.repo}} with your email as the user in the /rbac folder
Sample:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: your-handle
  namespace: your-handle
subjects:
- kind: User
  name: you@your-email.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: lab-admin
  apiGroup: rbac.authorization.k8s.io
```
4. Setup your configuration file for oauth

#### User Configuration
```yaml
- name: keycloak-oauth
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args:
      - oidc-login
      - get-token
      - --oidc-issuer-url=https://keycloak.sfxworks.net/auth/realms/mcsh
      - --oidc-client-id=account
      - --oidc-client-secret=da1768e7-d9ac-4980-887b-d8d11cdb207e
      command: kubectl
      env: null
```

#### Context Configuration
```yaml
- context:
    cluster: 757dev-lab
    user: keycloak-oauth
  name: 757dev-cluster
```

#### Cluster Configuration
```yaml
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJd01EVXdOREExTlRBd05Gb1hEVE13TURVd01qQTFOVEF3TkZvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBS0ZwClhZTXZRVlgzcXRpc2tBSDc1ZmlMSTNMdXkrQlBubkE2ZVhDRmkwMVZheEs5MXVnUWdhQWdCY0lKUmVhQTQrL2sKemJKay9DdUluZm1lRTdqVXdOZjJ6cEtGM2VEUk91VEU0dkc5TnVRZFpZcHV3UUo5QjZvS0VlSUFRakNkaTBxRApPUWdpM2lZM2ZWcEk0KzFnc2d3T1F4RjF2OFptb3ZsSktXcEcwejNrMzVHdGlicDM1TWRXRWdyUUl3ZTRFMkZ5CkFzNTdJL2dBdlNzYlpBRDJ3YUsyMityM2xHaXFYemhIeU02SDliMkxxVjN3YVR1RTc4R1JISGJZN0dyQmdva3EKT3VxRktXY05nOTdoN0pUaWpJcjJ4bGhsZ2JvV1I0elZESGJDQWttN25venZCQmc2SWs4WTFQSmIvS2tOaDdRWgpSenN0NE5LcmVlTDgxS3huai8wQ0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFJRG11NUM5b1BBYURhRzZJc1d2TzFzOHhHSEIKRlBsaW9LRUVsOXY4NU9aZ2xiSVNHei9SZEVaMHJ1TW8zenJGSTRNdkN5RUtybTNINnUvellYYU1Ya21YdUR5YQpFdEJBY3FPYlVNdUVUMW0rZStpRkUrdEtiL0RycElPTjZId2c5Q0Z1U01aMmI2RHNoUkhUbmkvb2xPNXBhZ0RSCkZIazZ6bVFOOXhnYUlZNytpZWNiMlpYQTloKzNJNlc3NHpUcFIvTHJJcmEzbzBRcUErYlJMZ1lieTBac0o1MEIKdkFEc3c5VDZhQkk2LzFKQWtxUXRtWE5VYmE4VS8rc1JtUWhxQllHNGIwajE2YUxwZVRHL2dHTGl1dWFsNzdHNwozb2tPR0J4TGFLMkdONmUyU0JHeVlHaGlLc0VOaUYvQ25LWEZWN1J2ejIzNnprdUNSak9SWnA4cFRYYz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    server: https://lab.k8s757.dev:6443
  name: 757dev-cluster
```