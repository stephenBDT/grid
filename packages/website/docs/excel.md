---
id: excel
title: Excel Import/Export
---
import { useState, useCallback } from 'react';
import Spreadsheet, { Sheet, defaultSheets } from "@rowsncolumns/spreadsheet";
import { excelToSheets, createExcelFileFromSheets } from '@rowsncolumns/export'

You will need to install `@rowsncolumns/export` npm package to add support for reading and writing excel file. Values that are preserved are

1. Cell values
1. Datatypes
1. Fill
1. Border
1. Frozen columns and rows
1. Merge cells

## Import from Excel


```jsx
import { excelToSheets, createExcelFileFromSheets } from '@rowsncolumns/export'
import Spreadsheet, { Sheet, defaultSheets } from "@rowsncolumns/spreadsheet";

const App = () => {
  const [ sheets, setSheets] = useState(defaultSheets)
  const handleFileSelect = (e) => {
    const getSheets = async (file) => {
      const newSheets = await excelToSheets({ file })
      setSheets(newSheets.sheets)
    }
    getSheets(e.target.files[0])
  }
  return (
    <>
      <input type="file" onChange={handleFileSelect} />
      <Spreadsheet
        sheets={sheets}
      />
    </>
  )
}
```

### Demo

export const App = () => {
  const [ sheets, setSheets] = useState(defaultSheets)
  const handleFileSelect = (e) => {
    const getSheets = async (file) => {
      const newSheets = await excelToSheets({ file })
      setSheets(newSheets.sheets)
    }
    getSheets(e.target.files[0])
  }
  return (
    <>
      <input type="file" onChange={handleFileSelect} />
      <Spreadsheet
        sheets={sheets}
      />
    </>
  )
}

<App />


## Export to excel

```jsx
const App = () => {
  const [ sheets, setSheets] = useState(defaultSheets)
  const handleExport = useCallback((sheets) => {
    const getSheets = async ({
      sheets
    }) => {
      const filename = 'download'
      const buffer = await createExcelFileFromSheets(sheets)
      const blob = new Blob([ buffer ], {type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' })
      const url = window.URL.createObjectURL(blob);
      const anchor = document.createElement('a');
      anchor.href = url;
      anchor.download = `${filename}.xlsx`;
      anchor.dispatchEvent(new MouseEvent('click'))
    }
    getSheets(sheets)
  }, [])
  return (
    <>
      <button onClick={() => handleExport({ sheets })}>Export to excel</button>
      <Spreadsheet
        sheets={sheets}
        onChange={setSheets}
      />
    </>
  );
};
```

### Demo

export const AppExport = () => {
  const [ sheets, setSheets] = useState(defaultSheets)
  const handleExport = useCallback((sheets) => {
    const getSheets = async ({
      sheets
    }) => {
      const filename = 'download'
      const buffer = await createExcelFileFromSheets(sheets)
      const blob = new Blob([ buffer ], {type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' })
      const url = window.URL.createObjectURL(blob);
      const anchor = document.createElement('a');
      anchor.href = url;
      anchor.download = `${filename}.xlsx`;
      anchor.dispatchEvent(new MouseEvent('click'))
    }
    getSheets(sheets)
  }, [])
  return (
    <>
      <button onClick={() => handleExport({ sheets })}>Export to excel</button>
      <Spreadsheet
        sheets={sheets}
        onChange={setSheets}
      />
    </>
  );
};

<AppExport />