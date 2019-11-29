# stateful mysql servive with phpmyadmin


oc create -f headlessmysql.yml
oc create -f mysqlsts.yml
oc create -f phpmyadmin-template.yaml
oc process phpmyadmin-template  | oc create -f -