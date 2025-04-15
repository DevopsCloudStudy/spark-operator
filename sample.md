```
apiVersion: v1
items:
- apiVersion: apps/v1
  kind: Deployment
  metadata:
    annotations:
      deployment.kubernetes.io/revision: "1"
      meta.helm.sh/release-name: spark-operator
      meta.helm.sh/release-namespace: spark-operator
    creationTimestamp: "2025-04-15T09:41:50Z"
    generation: 1
    labels:
      app.kubernetes.io/component: controller
      app.kubernetes.io/instance: spark-operator
      app.kubernetes.io/managed-by: Helm
      app.kubernetes.io/name: spark-operator
      app.kubernetes.io/version: 2.1.1
      helm.sh/chart: spark-operator-2.1.1
    name: spark-operator-controller
    namespace: spark-operator
    resourceVersion: "591925"
    uid: 5a1216d6-9e20-43af-ab48-1f055fd1aff1
  spec:
    progressDeadlineSeconds: 600
    replicas: 1
    revisionHistoryLimit: 10
    selector:
      matchLabels:
        app.kubernetes.io/component: controller
        app.kubernetes.io/instance: spark-operator
        app.kubernetes.io/name: spark-operator
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        annotations:
          prometheus.io/path: /metrics
          prometheus.io/port: "8080"
          prometheus.io/scrape: "true"
        creationTimestamp: null
        labels:
          app.kubernetes.io/component: controller
          app.kubernetes.io/instance: spark-operator
          app.kubernetes.io/name: spark-operator
      spec:
        automountServiceAccountToken: true
        containers:
        - args:
          - controller
          - start
          - --zap-log-level=info
          - --namespaces=default
          - --controller-threads=10
          - --enable-ui-service=true
          - --enable-metrics=true
          - --metrics-bind-address=:8080
          - --metrics-endpoint=/metrics
          - --metrics-prefix=
          - --metrics-labels=app_type
          - --metrics-job-start-latency-buckets=30,60,90,120,150,180,210,240,270,300
          - --leader-election=true
          - --leader-election-lock-name=spark-operator-controller-lock
          - --leader-election-lock-namespace=spark-operator
          - --workqueue-ratelimiter-bucket-qps=50
          - --workqueue-ratelimiter-bucket-size=500
          - --workqueue-ratelimiter-max-delay=6h
          - --driver-pod-creation-grace-period=10s
          - --max-tracked-executor-per-app=1000
          image: docker.io/kubeflow/spark-operator:2.1.1
          imagePullPolicy: IfNotPresent
          livenessProbe:
            failureThreshold: 3
            httpGet:
              path: /healthz
              port: 8081
              scheme: HTTP
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 1
          name: spark-operator-controller
          ports:
          - containerPort: 8080
            name: metrics
            protocol: TCP
          readinessProbe:
            failureThreshold: 3
            httpGet:
              path: /readyz
              port: 8081
              scheme: HTTP
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 1
          resources: {}
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
              - ALL
            privileged: false
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            seccompProfile:
              type: RuntimeDefault
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
          volumeMounts:
          - mountPath: /tmp
            name: tmp
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext:
          fsGroup: 185
        serviceAccount: spark-operator-controller
        serviceAccountName: spark-operator-controller
        terminationGracePeriodSeconds: 30
        volumes:
        - emptyDir:
            sizeLimit: 1Gi
          name: tmp
  status:
    availableReplicas: 1
    conditions:
    - lastTransitionTime: "2025-04-15T09:43:58Z"
      lastUpdateTime: "2025-04-15T09:43:58Z"
      message: Deployment has minimum availability.
      reason: MinimumReplicasAvailable
      status: "True"
      type: Available
    - lastTransitionTime: "2025-04-15T09:41:50Z"
      lastUpdateTime: "2025-04-15T09:43:58Z"
      message: ReplicaSet "spark-operator-controller-fb844b9b4" has successfully progressed.
      reason: NewReplicaSetAvailable
      status: "True"
      type: Progressing
    observedGeneration: 1
    readyReplicas: 1
    replicas: 1
    updatedReplicas: 1
- apiVersion: apps/v1
  kind: Deployment
  metadata:
    annotations:
      deployment.kubernetes.io/revision: "1"
      meta.helm.sh/release-name: spark-operator
      meta.helm.sh/release-namespace: spark-operator
    creationTimestamp: "2025-04-15T09:41:50Z"
    generation: 1
    labels:
      app.kubernetes.io/component: webhook
      app.kubernetes.io/instance: spark-operator
      app.kubernetes.io/managed-by: Helm
      app.kubernetes.io/name: spark-operator
      app.kubernetes.io/version: 2.1.1
      helm.sh/chart: spark-operator-2.1.1
    name: spark-operator-webhook
    namespace: spark-operator
    resourceVersion: "591913"
    uid: 8d613f06-6bb9-43b0-b1de-8d4e8aa7e4d1
  spec:
    progressDeadlineSeconds: 600
    replicas: 1
    revisionHistoryLimit: 10
    selector:
      matchLabels:
        app.kubernetes.io/component: webhook
        app.kubernetes.io/instance: spark-operator
        app.kubernetes.io/name: spark-operator
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        creationTimestamp: null
        labels:
          app.kubernetes.io/component: webhook
          app.kubernetes.io/instance: spark-operator
          app.kubernetes.io/name: spark-operator
      spec:
        automountServiceAccountToken: true
        containers:
        - args:
          - webhook
          - start
          - --zap-log-level=info
          - --namespaces=default
          - --webhook-secret-name=spark-operator-webhook-certs
          - --webhook-secret-namespace=spark-operator
          - --webhook-svc-name=spark-operator-webhook-svc
          - --webhook-svc-namespace=spark-operator
          - --webhook-port=9443
          - --mutating-webhook-name=spark-operator-webhook
          - --validating-webhook-name=spark-operator-webhook
          - --enable-metrics=true
          - --metrics-bind-address=:8080
          - --metrics-endpoint=/metrics
          - --metrics-prefix=
          - --metrics-labels=app_type
          - --leader-election=true
          - --leader-election-lock-name=spark-operator-webhook-lock
          - --leader-election-lock-namespace=spark-operator
          image: docker.io/kubeflow/spark-operator:2.1.1
          imagePullPolicy: IfNotPresent
          livenessProbe:
            failureThreshold: 3
            httpGet:
              path: /healthz
              port: 8081
              scheme: HTTP
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 1
          name: spark-operator-webhook
          ports:
          - containerPort: 9443
            name: webhook
            protocol: TCP
          - containerPort: 8080
            name: metrics
            protocol: TCP
          readinessProbe:
            failureThreshold: 3
            httpGet:
              path: /readyz
              port: 8081
              scheme: HTTP
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 1
          resources: {}
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
              - ALL
            privileged: false
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            seccompProfile:
              type: RuntimeDefault
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
          volumeMounts:
          - mountPath: /etc/k8s-webhook-server/serving-certs
            name: serving-certs
            subPath: serving-certs
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext:
          fsGroup: 185
        serviceAccount: spark-operator-webhook
        serviceAccountName: spark-operator-webhook
        terminationGracePeriodSeconds: 30
        volumes:
        - emptyDir:
            sizeLimit: 500Mi
          name: serving-certs
  status:
    availableReplicas: 1
    conditions:
    - lastTransitionTime: "2025-04-15T09:43:56Z"
      lastUpdateTime: "2025-04-15T09:43:56Z"
      message: Deployment has minimum availability.
      reason: MinimumReplicasAvailable
      status: "True"
      type: Available
    - lastTransitionTime: "2025-04-15T09:41:50Z"
      lastUpdateTime: "2025-04-15T09:43:56Z"
      message: ReplicaSet "spark-operator-webhook-7c648669b8" has successfully progressed.
      reason: NewReplicaSetAvailable
      status: "True"
      type: Progressing
    observedGeneration: 1
    readyReplicas: 1
    replicas: 1
    updatedReplicas: 1
kind: List
metadata:
  resourceVersion: ""
```
