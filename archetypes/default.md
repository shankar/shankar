---
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
author: {{ site.Params.author }}
email: {{ site.Params.email }}
date: '{{ .Date }}'
# Uncomment the below parameters to use them
#description:
#categories:
#   -
#tags:
#   -
#series:
#   -
#summary:
featured: false
draft: false
---