## Função para criar uma tabela Calendário - Power BI
### Esse trecho de código cria automáticamente uma dimensão de calendário pegando dados da sua tabela fato:
~~~
let
    DataMin = List.Min(#"RESERVAS POR DATA RETIRADA"[DATA RETIRADA]
),
    DataMax = List.Max(#"RESERVAS POR DATA RETIRADA"[DATA RETIRADA]

),
    QtdeDias = Duration.Days(DataMax - DataMin),
    ListaDatas = List.Dates(DataMin, QtdeDias, #duration(1, 0, 0,0)),
    #"Converted to Table" = Table.FromList(ListaDatas, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Changed Type" = Table.TransformColumnTypes(#"Converted to Table",{{"Column1", type date}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"Column1", "DATA"}}),
    #"Inserted Month Name" = Table.AddColumn(#"Renamed Columns", "Month Name", each Text.Upper(Text.Start(Date.MonthName([DATA]), 3)
), type text),
    #"Renamed Columns1" = Table.RenameColumns(#"Inserted Month Name",{{"Month Name", "SIGLA"}}),
    #"Inserted Day Name" = Table.AddColumn(#"Renamed Columns1", "Day Name", each Text.Upper(Text.Start(Date.DayOfWeekName([DATA]), 3))
, type text),
    #"Renamed Columns2" = Table.RenameColumns(#"Inserted Day Name",{{"Day Name", "DIA"}}),
    #"Inserted Day" = Table.AddColumn(#"Renamed Columns2", "Day", each Date.Day([DATA]), Int64.Type),
    #"Renamed Columns3" = Table.RenameColumns(#"Inserted Day",{{"Day", "COD.DIA"}}),
    #"Inserted Month Name1" = Table.AddColumn(#"Renamed Columns3", "Month Name", each Date.MonthName([DATA]), type text),
    #"Inserted Uppercased Text" = Table.AddColumn(#"Inserted Month Name1", "UPPERCASE", each Text.Upper([Month Name]), type text)
in
    #"Inserted Uppercased Text"
~~~
