Для решения данной задачи мы можем создать манифест для Kubernetes:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 4
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app-image
        resources:
          requests:
            cpu: 0.5           # переопределение ресурсов CPU для первых запросов
            memory: 128Mi
          limits:
            cpu: 1             # предельное использование CPU
            memory: 128Mi
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    tolerations:                # толерансы для узлов
    - key: zone
      operator: Exists
    - key: workload
      operator: Equal
      value: high
      effect: NoSchedule


В данном манифесте мы создаем деплоймент с 4 репликами, задаем ресурсы CPU и памяти для контейнера, а также предельное использование CPU. Используем RollingUpdate стратегию для обновления подов без перерывов в обслуживании. Также включаем толерансы для узлов, чтобы оптимизировать использование ресурсов. Мы можем также использовать горизонтальное масштабирование для автоматического увеличения или уменьшения количества подов в зависимости от текущей нагрузки на приложение. 

