
# Ingeniero de Datos de Estadios de Fútbol

Este proyecto basado en Python rastrea datos de https://en.wikipedia.org/wiki/List_of_association_football_stadiums_by_capacity utilizando Azure Databricks, los limpia y los envía a Azure Data Lake para su procesamiento. Más tarde, visualizaremos los datos a través de Power BI para obtener información de negocio.

## Tabla de contenido

1. [Arquitectura del Sistema](#arquitectura-del-sistema)
2. [WebScrapping (No es necerario en Databricks)](./scrapping.ipynb)
3. [Requisitos](#requisitos)
4. [Rastreo Web](#rastreo-web)
6. [Azure](#azure)
7. [Power BI](#power-bi)
8. [Video](#video)


## Arquitectura del Sistema
![imagen (17)](https://github.com/user-attachments/assets/9823a0b2-f11d-4139-829b-26f84db7021e)


## Requisitos
**Accounts:**
  - Databricks
  - Power BI
  - Microsoft

**Technologies:**
  - Azure Datalake Storage Gen 2
  - Azure Databricks
  - Python 3.12
  - Pip
  - Power BI
  - Azure HDInsight
  - Azure Data Factory

## Web Crawling
1. Abrimos una terminal dentro de Databricks. Ejecutamos estos comandos:
```bash
touch requirements.txt
nano requirements.txt
```
2. Dentro de requirements.txt, agregamos el contenido del archivo: Requirements/requirements.txt
```bash
pip install -r requirements.txt
```
Esto instalará todas las bibliotecas y dependencias dentro de requirements.txt 
NOTA: hay más bibliotecas y dependencias de las que necesita.

3. Al ejecutar el notebook, extraerá y limpiará todos los datos de los estadios de Wikipedia, guardándolos posteriormente en un Dataframe.

4. Finalmente, el dataframe se guarda como un csv dentro de un contenedor en un datalake. Debe cambiar la Clave SSA y el nombre del contenedor por los de su almacenamiento de Datalake.

## Azure
1. Crear una Storage Account(Cuenta de Almacenamiento)

![1](https://github.com/user-attachments/assets/c0f3bec2-4b82-432f-9d4d-64e8bdb9b706)

2. Realizar la siguiente configuración:

![2](https://github.com/user-attachments/assets/1becd5e9-4e24-48b1-8c32-24008f430603)

3. En avanzado, seleccione la opción --> Enable hierarchical(Habilitar jerárquicia)

![3](https://github.com/user-attachments/assets/55fd5bde-bbd3-4353-bd79-1f23f28345a8)

4. Cuando la storage account se haya creado, vaya a Data storage/Containers(Almacenamiento de datos/Contenedores)

![4](https://github.com/user-attachments/assets/e3d503d0-bbf7-4511-93b7-4aa97d303282)

5. Crear un nuevo container(contenedor)

![imagen (3)](https://github.com/user-attachments/assets/cfa23ab8-9197-40e4-ad9d-73e188657c4f)

6. Abrir el contenedor

![5](https://github.com/user-attachments/assets/284616c5-a09d-43de-99ee-482007e00422)

7. En el contenedor, Agregar nuevo Directorio

![5 1](https://github.com/user-attachments/assets/5734586e-c132-4010-a409-127bbfd6a2ad)

8. Ya hemos creado el Directorio

![5 2](https://github.com/user-attachments/assets/15fe9b07-41c5-4e76-a139-42c5a0e28e3c)

9. Volver a la storage account y abrir Access key. Debe copiar la cadena de conexión para cargar el csv en data factory. Esto tambien se necesita para el ultimo pasi de scrapping, ya que necesitamos pegar la key para hacer la conexión a Azure

![imagen (5)](https://github.com/user-attachments/assets/35e5afcd-a3db-4584-9336-7edcd5402513)

10. Crear un servicio de Data Factories

![9](https://github.com/user-attachments/assets/136ee279-ab95-401f-8ea1-31890bd65904)

11. Configurar las opciones

![10](https://github.com/user-attachments/assets/fb02bb7f-fb3a-4a6a-8ff2-246b619eabcf)

12. Cuando se crea el servicio, Iniciar Azure Data Factory Studio

![11](https://github.com/user-attachments/assets/2bd7ecc4-c319-480d-92b2-e4e38d108dc4)

13. Crear una nueva canalización (pipeline)

![image](https://github.com/user-attachments/assets/78fd6b95-e753-4c96-abe7-c3972042dd1e)

14. Abrir la canalización y seleccionar la opción -> Copiar datos

![2](https://github.com/user-attachments/assets/637c576b-c79d-4d2b-9ec5-94bba46bf843)

15. En origen (source), crear un nuevo conjunto de datos de origen (source dataset)

![3](https://github.com/user-attachments/assets/c666e650-db0c-4654-927f-78edac8ff33f)

16. Seleccionar Azure Data Lake Storage Gen 2

![4](https://github.com/user-attachments/assets/395bc48e-35b5-4633-ac72-64cfa2c6e207)

17. Seleccionar la opción de formato csv

![5](https://github.com/user-attachments/assets/4d0b490e-0e04-4f9f-87d6-9be66a7d33b1)

18. Agregar un nuevo servidor vinculado (linked server) para conectar con los datos del contenedor

![6](https://github.com/user-attachments/assets/951c20e0-f9b1-4e97-b0dc-80d6985c0a92)

19. En las opciones, seleccionar nuestra storage account 

![7](https://github.com/user-attachments/assets/36a52843-3af1-4319-9aac-0c2cf0cf5bcf)

20. En propiedades, seleccionar la ruta donde tiene el csv en el contenedor creado

![8](https://github.com/user-attachments/assets/de5f87ec-6e8d-4750-abd1-1adefca4b4b1)

21. Vista previa de los datos para verificar que se hayan cargado correctamente

![9](https://github.com/user-attachments/assets/399fd52f-4f7b-49e3-afd9-2a2064eafd03)

22. Verificamos que estén cargados

![imagen (8)](https://github.com/user-attachments/assets/88bb6f0c-e425-48f7-a8d2-160a7878e9b6)

23. En destino (sink), crear un nuevo conjunto de datos

![11](https://github.com/user-attachments/assets/b4bd5658-085b-468e-bb24-d7ad2335b99e)

24. Seleccionar Azure Data Lake Storage Gen 2

![12](https://github.com/user-attachments/assets/12f2721f-64ee-4441-8112-44faef79bba6)

25. Especificar la ruta para cargar los datos en el directorio -> listo


![14](https://github.com/user-attachments/assets/4640942f-9b86-4489-a06f-a5567c2c167e)


26. Crear un servicio de Azure Synapse Analytics


![imagen (14)](https://github.com/user-attachments/assets/1ed7cba8-3e2a-4a15-8273-da82add1e8c1)

27. Configurar las opciones


![imagen (15)](https://github.com/user-attachments/assets/ed10669d-6737-41db-8b98-44884a862400)


28. Abrir Azure Synapse Studio


![1](https://github.com/user-attachments/assets/5f84b9fe-833d-4dc7-bf1b-f9fa80c9a84d)


29. Iniciar Azure Synapse Studio


![2](https://github.com/user-attachments/assets/8afda5f8-ebdd-477f-9f04-186e4e1e6279)


30. En Synapse, Crear una Lake database



![3](https://github.com/user-attachments/assets/96b7210d-5689-4807-8491-7b981a221e6d)


31. Crear una tabla a partir de data lake


![4](https://github.com/user-attachments/assets/b830b225-a82a-45bc-860b-146ae632e424)

32. Configurar el nombre de la tabla y el servicio vinculado(linked served) 


![5](https://github.com/user-attachments/assets/16a2952e-2c19-49a2-93dd-e77747a644e8)


33. Crear la tabla externa


![6](https://github.com/user-attachments/assets/d5d4582a-8e99-42f8-b5b3-b3c438dae056)

34. Publicar los cambios



![7](https://github.com/user-attachments/assets/741a9e87-eacf-4e78-872b-f539ca409d67)


35. Crear un Sql Synapse



![8](https://github.com/user-attachments/assets/01132c92-99bd-4e13-a92b-a54190ab27a7)

36. Seleccionar nuestra base de datos



![9](https://github.com/user-attachments/assets/c4a81e4b-4de7-4024-ba6a-3fd354af75e5)


37. Ejecutar una consulta para seleccionar la capacidad promedio del estadio por región



![10](https://github.com/user-attachments/assets/abbb8d80-a32c-4242-a190-e2b47e1ccb80)


38. Consulta para filtrar el estadio con capacidad máxima por región



![11](https://github.com/user-attachments/assets/82a1e33a-bb8b-49cb-bf29-1df4e203632c)

39. Consulta de estadios con una capacidad superior a un cierto valor en un país específico




![12](https://github.com/user-attachments/assets/93ebeaab-c45f-4f93-964e-15120a97991c)


## Power BI

1. Conexión
Ingresar a Power BI e ir a obtener datos, luego hacer clic en:


![image](https://github.com/user-attachments/assets/67a29d8e-350c-4baf-ad86-73da062fa66d)


Y seleccionamos nuestra tabla de datos dentro de Synapse:



![image](https://github.com/user-attachments/assets/09c71b34-1d6d-4d63-a3ab-1563fb16e957)

2. Los KPIs estan en el .pbix del repositorio
