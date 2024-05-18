# Test Case Analysis

Se define los modelos a ocupar, primero ocuparemos DataModel
```
public class DataModel {
    private String data;
    private String processedSuccessfully;

    public DataModel(String data) {
        this.data = data;
    }
    public String getData() {
        return data;
    }

    public void setData(String data) {
        this.data = data;
    }

    public String getProcessedSuccessfully() {
        return processedSuccessfully;
    }

    public void setProcessedSuccessfully(String processedSuccessfully) {
        this.processedSuccessfully = processedSuccessfully;
    }
}
```

Tambien ocuparemos UiComponentModel
```
public class UiComponentModel {
    private Integer size;

    public Integer getSize() {
        return size;
    }

    public void setSize(Integer size) {
        this.size = size;
    }

    public String adjustToScreenSize(Integer screenSize) {
        size = screenSize;
        return "UI should adjust to width of " + screenSize + " pixels";
    }
}
```

Y ahora definimos el service donde estaran las funciones que probaremos
```
public class ExamplesService {

    public String authenticate(String username, String password) {
        if (username.equals("validUser") && password.equals("validPass")) {
            return "Should succeed with correct credentials";
        } else {
            return "Should fail with wrong credentials";
        }
    }

    public void processData(DataModel dataModel) throws Exception {
        if (dataModel.getData().equalsIgnoreCase("some error")) {
            throw new Exception("Data processing error");
        }
        dataModel.setProcessedSuccessfully("Data should be processed successfully");
    }

    public UiComponentModel setupUiComponent(Integer size) throws Exception {
        UiComponentModel uiComponentModel = new UiComponentModel();
        uiComponentModel.setSize(size);
        if(size < 0) {
            throw new Exception("Size cannot be negative");
        }
        return uiComponentModel;
    }

}
```

## Scenario 1: User Authentication Tests

Para el primer escenario creamos 3 test que nos ayuden a validar cuando la autenticacion es correcta, cuando es incorrecta por el usuario invalido o por el password invalido
```
private final ExamplesService examplesService = new ExamplesService();

    @Test
    public void testUserAuthentication_correctCredentials() {
        String username = "validUser";
        String password = "validPass";
        String expectedMessage = "Should succeed with correct credentials";
        String returnMessage = examplesService.authenticate(username, password);
        assertEquals(expectedMessage, returnMessage);
    }

    @Test
    public void testUserAuthentication_wrongCredentials_user() {
        String username = "invalidUser";
        String password = "validPass";
        String expectedMessage = "Should fail with wrong credentials";
        String returnMessage = examplesService.authenticate(username, password);
        assertEquals(expectedMessage, returnMessage);
    }

    @Test
    public void testUserAuthentication_wrongCredentials_password() {
        String username = "validUser";
        String password = "invalidPass";
        String expectedMessage = "Should fail with wrong credentials";
        String returnMessage = examplesService.authenticate(username, password);
        assertEquals(expectedMessage, returnMessage);
    }
```

## Scenario 2: Data Processing Functions

Para el segundo escenario creamos dos test donde validamos que la data se proceso correctamente y el otro donde validaremos que en dado caso de no ser procesada se mande una excepcion
```
    private final ExamplesService examplesService = new ExamplesService();

    public DataModel fetchData() {
        return new DataModel("some");
    }

    @Test
    public void testProcessData_successfullyProcessing() throws Exception {
        DataModel dataModel = fetchData();
        examplesService.processData(dataModel);
        String expectedMessage = "Data should be processed successfully";
        assertEquals(expectedMessage, dataModel.getProcessedSuccessfully());
    }

    @Test
    public void testProcessData_errorProcessing() {
        DataModel dataModel = new DataModel("some error");

        Exception exception = Assertions.assertThrows(Exception.class, () -> {
            examplesService.processData(dataModel);
        });
        String expectedMessage = "Data processing error";
        assertEquals(expectedMessage, exception.getMessage(), "Should handle processing errors");
    }
```

## Scenario 3: UI Responsiveness

Para el tercer escenario creamos dos tests donde elprimero valida la correcta asignacion del valor de configuracion inicial, y el otro donde validamos el uso de valores negativos para el tamaño
```
    private final ExamplesService examplesService = new ExamplesService();

    @Test
    public void testSetupUiComponent_successResize() throws Exception {
        Integer originalSize = 10;
        UiComponentModel uiComponentModel = examplesService.setupUiComponent(originalSize);
        assertEquals(originalSize, uiComponentModel.getSize());
        assertEquals("UI should adjust to width of 1024 pixels", uiComponentModel.adjustToScreenSize(1024));
        assertEquals(1024, uiComponentModel.getSize());
    }

    @Test
    public void testSetupUiComponent_failResizeWithNegative() {
        Integer originalSize = -100;
        Exception exception = Assertions.assertThrows(Exception.class, () -> {
            examplesService.setupUiComponent(originalSize);
        });
        String expectedMessage = "Size cannot be negative";
        assertEquals(expectedMessage, exception.getMessage());
    }
```

Adiconalmente creamos un test parametrizado en el que probamos que el mensaje se mostrara correctamente sin importar el valor asignado
```
    private final ExamplesService examplesService = new ExamplesService();

    @Test
    public void testSetupUiComponent_successOriginalResize() throws Exception {
        Integer originalSize = 10;
        UiComponentModel uiComponentModel = examplesService.setupUiComponent(originalSize);
        assertEquals(originalSize, uiComponentModel.getSize());
    }
```
