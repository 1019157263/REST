
#  1.在app目录下创建一个serializers.py

目录：
> appname/serializers.py

```
from django.contrib.auth.models import User, Group
from rest_framework import serializers


class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ('url', 'username', 'email', 'groups')


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ('url', 'name')
```
#  2.在app目录下的views.py输入以下代码：
 目录：
  > appname/views.py

```
from django.contrib.auth.models import User, Group
from rest_framework import viewsets
from tutorial.quickstart.serializers import UserSerializer, GroupSerializer


class UserViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows users to be viewed or edited.
    """
    queryset = User.objects.all().order_by('-date_joined')
    serializer_class = UserSerializer


class GroupViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows groups to be viewed or edited.
    """
    queryset = Group.objects.all()
    serializer_class = GroupSerializer
```

#  3.配置api-url路由子在工程目录下的urls.py
目录： > projectname/urls.py

```
from django.conf.urls import url, include
*from rest_framework import routers*
from tutorial.quickstart import views

router = routers.DefaultRouter()
router.register(r'users', views.UserViewSet)
router.register(r'groups', views.GroupViewSet)

 Wire up our API using automatic URL routing.
 Additionally, we include login URLs for the browsable API.
urlpatterns = [
    *url(r'^', include(router.urls)),*
    *url(r'^api-auth/', include('rest_framework.urls', *namespace='rest_framework'))
```