# How to set keyboard type for a column in Flutter DataTable (SfDataGrid)?

In this article, we will show how to set keyboard type for a column in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with the necessary properties. By default, SfDataGrid does not load any widget when a cell enters edit mode. To enable editing, you must return the appropriate widget using the [buildEditWidget](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource/buildEditWidget.html) method in the [DataGridSource](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource-class.html) class.

In this case, we use a TextField widget for editing. Based on the column name, we can determine whether a numeric or alphabetic keyboard should appear for value entry. The [keyboardType](https://api.flutter.dev/flutter/material/TextField/keyboardType.html) property of the TextField specifies the type of keyboard to display (e.g., TextInputType.number for numeric input).

```dart
  @override
  Widget? buildEditWidget(DataGridRow dataGridRow,
      RowColumnIndex rowColumnIndex, GridColumn column, CellSubmit submitCell) {
    final String displayText = dataGridRow
            .getCells()
            .firstWhereOrNull((DataGridCell dataGridCell) =>
                dataGridCell.columnName == column.columnName)
            ?.value
            ?.toString() ??
        '';

    newCellValue = null;

    final bool isNumericType =
        column.columnName == 'ID' || column.columnName == 'Salary';

    return Container(
      padding: const EdgeInsets.all(8.0),
      alignment: isNumericType ? Alignment.centerRight : Alignment.centerLeft,
      child: TextField(
        autofocus: true,
        controller: editingController..text = displayText,
        textAlign: isNumericType ? TextAlign.right : TextAlign.left,
        decoration: const InputDecoration(
          contentPadding: EdgeInsets.fromLTRB(0, 0, 0, 8.0),
        ),
        keyboardType:
            isNumericType ? TextInputType.number : TextInputType.text,
        onChanged: (String value) {
          if (value.isNotEmpty) {
            if (isNumericType) {
              newCellValue = int.parse(value);
            } else {
              newCellValue = value;
            }
          } else {
            newCellValue = null;
          }
        },
        onSubmitted: (String value) {
          submitCell();
        },
      ),
    );
  }
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-set-keyboard-type-for-a-column-in-Flutter-DataTable).