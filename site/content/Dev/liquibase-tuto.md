---
title: "Liquibase"
date: 2018-10-15T10:25:04+02:00
draft: false

---

## Initialisation
Liquibase, dans JHipster

J'ai par exemple un fichier csv

*init-materiel.csv*
```csv
id, nom, marque
1,PC 1, Dell
2,PC2,Asus
3,PC3,Inspiron
```

et je le charge en utilisant un nouveau changeset

.added_machin.xml

```xml
<changeSet id="20180111162213-1" author="pdelaby">
  <loadUpdateData  catalogName="materiel"
          encoding="UTF-8"
          file="init-materiel.csv"
          relativeToChangelogFile="true"
          separator=","
          primaryKey="id"
          tableName="materiel">
      <column name="id" type="numeric"/>
      <column name="nom" type="string"/>
      <column name="marque" type="string"/>
  </loadUpdateData >
</changeSet>
```
