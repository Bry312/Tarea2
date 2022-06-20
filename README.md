# Tarea2
Tablas de Delitos
### Tabla de Columnas  
```{r # Importación de datos}
estadisticas_policiales <-
 readxl::read_excel ("C:/Users/Bryan/OneDrive/Desktop/Procesamiento de Datos/Tarea2/estaisticas_policiales_2021/estadisticaspoliciales2021.xls"
  )

# Transformación de datos 
estadisticas_policiales <-
  estadisticas_policiales %>%     
  select(Delito= Delito, Fecha, Victima, Edad, Genero = Genero, Provincia, Canton) %>%  
  mutate(Fecha = as.Date(Fecha, format = "%d/%m/%Y"))

# Visualización de datos en formato tabular
estadisticas_policiales %>%  
  datatable(options = list(
    pageLength = 5,
    language = list(url = '//cdn.datatables.net/plug-ins/1.10.11/i18n/Spanish.json')
  ))
```

### Gráfica de barras simple  

```{r ggplotly - Gráfico de barras simples}
ggplot2_barras_proporcion <-
  estadisticas_policiales %>%  
  ggplot(aes(x = Delito, y = stat(count), group = 100)) +
  geom_bar() +
  ggtitle("Tipos de Delitos") +
  xlab("Delito") +
  ylab("Fecha") +
  theme_minimal()

ggplotly(ggplot2_barras_proporcion) %>% config(locale = 'es')
```


### Gráfica de Delitos por Mes  

```{r}
ggplot2_histograma_estadisticas_policiales <-
  estadisticas_policiales %>%  
  ggplot(aes(x = Fecha)) +
  geom_histogram(binwidth = 80) + 
  ggtitle("Delitos por Mes") +
  xlab("Fecha") +
  ylab("Cantidad de Delitos") +
  theme_minimal()

ggplotly(ggplot2_histograma_estadisticas_policiales) %>% config(locale = 'es')  
```
### Gráfico de barras Apiladas  

```{r Gráfico de barras apiladas de cantidades}
ggplot2_barras_apiladas_cantidad <-
  estadisticas_policiales %>%
  ggplot(aes(x = Delito, fill = Genero)) +
  geom_bar() +
  ggtitle("Proporcion de delito por genero") +
  xlab("Genero") +
  ylab("Cantidad") +
  labs(fill = "Delito") +
  theme_minimal()

ggplotly(ggplot2_barras_apiladas_cantidad) %>% config(locale = 'es')
```


### Gráfico de barras por cantón  

```{r}
barras <- filter(estadisticas_policiales, grepl('HEREDIA|ALAJUELA|CARTAGO|SAN JOSE', Canton))

Delitos_Canton <-
  ggplot(data = barras, aes(x = Canton)) +
  geom_bar() +
  ggtitle("Delitos por Canton") +
  xlab("Canton") +
  ylab("Cantidad de Delitos") +
  theme_minimal()

ggplotly(Delitos_Canton) %>% config(local = 'es')
