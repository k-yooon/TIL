# 유용한 TIP

- 나이파이 파일 업로드 불가시
  - curl -XPOST http://xxx.xxx.xx.xx:xxxx/nifi-api/process-groups/11111111-2222222/templates/upload -k -v -F template=@/home/template.xml
