if not os.path.exists('../tilt-go-api-k8s'):
  fail('Please "git clone" the api repo in ../tilt-go-api-k8s!')

if not os.path.exists('../tilt-react-ts-k8s'):
  fail('Please "git clone" the react app repo in ../tilt-react-ts-k8s!')

print('All repos are present importing configs')

include('../tilt-go-api-k8s/Tiltfile')
include('../tilt-react-ts-k8s/Tiltfile')

# load traefik ingress controller from helm
load('ext://helm_remote', 'helm_remote')
helm_remote('traefik',
            repo_name='traefik',
            repo_url='https://helm.traefik.io/traefik',
            set=["additionalArguments={--log.level=DEBUG,--api.dashboard=true,--api.debug=true,--api.insecure=true}"]
            )

# expose traefik dashboard and setup ingress rules
k8s_yaml('k8s/traefik-ingress.yaml')
k8s_resource(
  objects=['app-api-ingress:IngressRoute:default'],
  new_name='traefik-ingress'
)
# kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
# helm_remote(chart,
# repo_url='',
# repo_name='',
# release_name='',
# namespace='',
# version='',
# username='', 
# password='',
# values=[],
# set=[])