# Cloudeka Helm Chart Base Templates

## Table of Contents

1. [Chart.yaml](#chartyaml)
2. [_helpers.tpl](#helperstpl)
3. [Deployment](#deployment)
4. [Service](#service)
5. [PVC](#pvc)
6. [ConfigMap](#configmap)
7. [HPA](#hpa)
8. [Ingress](#ingress)
9. [ServiceAccount](#serviceaccount)
10. [values.yaml Structure](#valuesyaml-structure)

---

## Chart.yaml

```yaml
apiVersion: v2
name: {{ CHART_NAME }}
description: {{ CHART_DESCRIPTION }}
type: application
version: 0.1.0
appVersion: "1.0.0"
```

## _helpers.tpl

```yaml
{{- define "chart.fullname" -}}
{{- .Values.app | default .Chart.Name | trunc 63 | trimSuffix "-" -}}
{{- end -}}

{{- define "chart.labels" -}}
app: {{ .Values.app }}
chart: {{ .Chart.Name }}-{{ .Chart.Version }}
release: {{ .Release.Name }}
{{- end -}}

{{- define "chart.selectorLabels" -}}
app: {{ .Values.app }}
{{- end -}}
```

## Deployment

Cloudeka mandatory: securityContext with non-root user.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.app }}
  labels:
    {{- include "chart.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      {{- include "chart.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      labels:
        {{- include "chart.selectorLabels" . | nindent 8 }}
    spec:
      {{- if .Values.serviceAccount }}
      {{- if .Values.serviceAccount.name }}
      serviceAccountName: {{ .Values.serviceAccount.name }}
      {{- end }}
      {{- end }}
      containers:
      - name: {{ .Values.app }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        imagePullPolicy: {{ .Values.image.pullPolicy | default "IfNotPresent" }}
        {{- if .Values.command }}
        command:
          {{- toYaml .Values.command | nindent 10 }}
        {{- end }}
        {{- if .Values.args }}
        args:
          {{- toYaml .Values.args | nindent 10 }}
        {{- end }}
        ports:
        - containerPort: {{ .Values.containerPort | default 8080 }}
          protocol: TCP
        {{- if .Values.env }}
        env:
          {{- toYaml .Values.env | nindent 10 }}
        {{- end }}
        {{- if .Values.envFrom }}
        envFrom:
          {{- toYaml .Values.envFrom | nindent 10 }}
        {{- end }}
        resources:
          limits:
            cpu: {{ .Values.resources.limits.cpu }}
            memory: {{ .Values.resources.limits.memory }}
            {{- if .Values.resources.limits.ephemeralStorage }}
            ephemeral-storage: {{ .Values.resources.limits.ephemeralStorage }}
            {{- end }}
          requests:
            cpu: {{ .Values.resources.requests.cpu }}
            memory: {{ .Values.resources.requests.memory }}
            {{- if .Values.resources.requests.ephemeralStorage }}
            ephemeral-storage: {{ .Values.resources.requests.ephemeralStorage }}
            {{- end }}
        securityContext:
          runAsUser: 1000
          runAsNonRoot: true
          allowPrivilegeEscalation: false
        {{- if .Values.probes }}
        {{- if .Values.probes.liveness }}
        livenessProbe:
          httpGet:
            path: {{ .Values.probes.liveness.path }}
            port: {{ .Values.probes.liveness.port }}
          initialDelaySeconds: {{ .Values.probes.liveness.initialDelaySeconds | default 15 }}
          periodSeconds: {{ .Values.probes.liveness.periodSeconds | default 20 }}
          failureThreshold: {{ .Values.probes.liveness.failureThreshold | default 3 }}
        {{- end }}
        {{- if .Values.probes.readiness }}
        readinessProbe:
          httpGet:
            path: {{ .Values.probes.readiness.path }}
            port: {{ .Values.probes.readiness.port }}
          initialDelaySeconds: {{ .Values.probes.readiness.initialDelaySeconds | default 10 }}
          periodSeconds: {{ .Values.probes.readiness.periodSeconds | default 10 }}
          failureThreshold: {{ .Values.probes.readiness.failureThreshold | default 3 }}
        {{- end }}
        {{- end }}
        {{- if .Values.volumeMounts }}
        volumeMounts:
          {{- toYaml .Values.volumeMounts | nindent 10 }}
        {{- end }}
      {{- if .Values.volumes }}
      volumes:
        {{- toYaml .Values.volumes | nindent 8 }}
      {{- end }}
      {{- if .Values.affinity }}
      affinity:
        {{- toYaml .Values.affinity | nindent 8 }}
      {{- end }}
      {{- if .Values.tolerations }}
      tolerations:
        {{- toYaml .Values.tolerations | nindent 8 }}
      {{- end }}
```

## Service

```yaml
{{- if .Values.service.enabled }}
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.service.name | default .Values.app }}
  labels:
    {{- include "chart.labels" . | nindent 4 }}
  {{- if .Values.service.annotations }}
  annotations:
    {{- toYaml .Values.service.annotations | nindent 4 }}
  {{- end }}
spec:
  type: {{ .Values.service.type | default "ClusterIP" }}
  ports:
  {{- range .Values.service.ports }}
  - name: {{ .name }}
    port: {{ .port }}
    protocol: {{ .protocol | default "TCP" }}
    targetPort: {{ .targetPort }}
  {{- end }}
  selector:
    {{- include "chart.selectorLabels" . | nindent 4 }}
  sessionAffinity: {{ .Values.service.sessionAffinity | default "None" }}
{{- end }}
```

## PVC

```yaml
{{- if .Values.pvc.enabled }}
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: {{ .Values.pvc.name | default .Values.app }}
  labels:
    {{- include "chart.labels" . | nindent 4 }}
spec:
  accessModes:
  - {{ .Values.pvc.accessMode | default "ReadWriteOnce" }}
  resources:
    requests:
      storage: {{ .Values.pvc.size }}
  storageClassName: {{ .Values.pvc.storageClassName | default "storage-nvme-c1" }}
  volumeMode: Filesystem
{{- end }}
```

## ConfigMap

```yaml
{{- if .Values.configMap }}
{{- if .Values.configMap.enabled }}
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Values.configMap.name | default .Values.app }}
  labels:
    {{- include "chart.labels" . | nindent 4 }}
data:
  {{- toYaml .Values.configMap.data | nindent 2 }}
{{- end }}
{{- end }}
```

## HPA

```yaml
{{- if .Values.autoscaling }}
{{- if .Values.autoscaling.enabled }}
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: {{ .Values.app }}
  labels:
    {{- include "chart.labels" . | nindent 4 }}
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: {{ .Values.app }}
  minReplicas: {{ .Values.autoscaling.minReplicas | default 1 }}
  maxReplicas: {{ .Values.autoscaling.maxReplicas | default 5 }}
  metrics:
  {{- if .Values.autoscaling.targetCPUUtilizationPercentage }}
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: {{ .Values.autoscaling.targetCPUUtilizationPercentage }}
  {{- end }}
  {{- if .Values.autoscaling.targetMemoryUtilizationPercentage }}
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: {{ .Values.autoscaling.targetMemoryUtilizationPercentage }}
  {{- end }}
{{- end }}
{{- end }}
```

## Ingress

```yaml
{{- if .Values.ingress }}
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Values.ingress.name | default .Values.app }}
  labels:
    {{- include "chart.labels" . | nindent 4 }}
  {{- if .Values.ingress.annotations }}
  annotations:
    {{- toYaml .Values.ingress.annotations | nindent 4 }}
  {{- end }}
spec:
  {{- if .Values.ingress.ingressClassName }}
  ingressClassName: {{ .Values.ingress.ingressClassName }}
  {{- end }}
  {{- if .Values.ingress.tls }}
  tls:
    {{- toYaml .Values.ingress.tls | nindent 4 }}
  {{- end }}
  rules:
  {{- range .Values.ingress.rules }}
  - host: {{ .host }}
    http:
      paths:
      {{- range .paths }}
      - path: {{ .path }}
        pathType: {{ .pathType | default "Prefix" }}
        backend:
          service:
            name: {{ .serviceName }}
            port:
              number: {{ .servicePort }}
      {{- end }}
  {{- end }}
{{- end }}
{{- end }}
```

## ServiceAccount

```yaml
{{- if .Values.serviceAccount }}
{{- if .Values.serviceAccount.create }}
apiVersion: v1
kind: ServiceAccount
metadata:
  name: {{ .Values.serviceAccount.name | default .Values.app }}
  labels:
    {{- include "chart.labels" . | nindent 4 }}
  {{- if .Values.serviceAccount.annotations }}
  annotations:
    {{- toYaml .Values.serviceAccount.annotations | nindent 4 }}
  {{- end }}
{{- end }}
{{- end }}
```

## values.yaml Structure

Base values structure (non-GPU):

```yaml
app: my-app
replicaCount: 1

image:
  repository: dekaregistry.cloudeka.id/namespace/image-name
  tag: latest
  pullPolicy: IfNotPresent

containerPort: 8080

service:
  enabled: true
  name: ""
  type: ClusterIP
  ports:
    - name: http
      port: 80
      targetPort: 8080
      protocol: TCP
  sessionAffinity: None

resources:
  limits:
    cpu: "2"
    memory: "4Gi"
  requests:
    cpu: "1"
    memory: "2Gi"

pvc:
  enabled: false
  name: ""
  storageClassName: storage-nvme-c1
  size: 10Gi
  accessMode: ReadWriteOnce

probes:
  liveness:
    path: /healthz
    port: 8080
    initialDelaySeconds: 15
    periodSeconds: 20
    failureThreshold: 3
  readiness:
    path: /ready
    port: 8080
    initialDelaySeconds: 10
    periodSeconds: 10
    failureThreshold: 3

env: []
# - name: ENV_VAR
#   value: "value"
```
