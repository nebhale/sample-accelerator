custom_build('artifact-registry/project-name',
  'tanzu apps workload apply -f config/workload.yaml --local-path=. --yes && \
    while [[ $(kubectl get ksvc project-name -o \'jsonpath={..status.conditions[?(@.type=="Ready")].status}\') != "True" ]]; do echo "waiting for ksvc" && sleep 10; done',
  ['pom.xml', './target/classes'],
  live_update = [
    sync('./bin/main', '/workspace/BOOT-INF/classes')
  ],
  skips_local_docker=True
)

k8s_yaml('./config/workload.yaml')
k8s_kind('Workload', image_json_path='{.metadata.run-image}')
k8s_resource(workload='project-name', extra_pod_selectors=[{'serving.knative.dev/service':'project-name'}])
