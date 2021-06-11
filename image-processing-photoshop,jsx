#target photoshop

function main(){
    var doc = app.activeDocument; // Reference the active file
    var saveWhere = "SAME";
    var signatureName;
    var toyName;
    app.preferences.rulerUnits - Units.PIXELS;
    app.preferences.typeUnits - Units.POINTS;
    // ==========================================================
    // ==================== DIALOG BOX MENU =====================
    // ==========================================================
    var wSelect = new Window( "dialog", "3AFans.com Image Hacks" );
        wSelect.btnPnl = wSelect.add( "panel", undefined, "3AFans.com Image Processes" );
        
    btnNewGalleryPic = wSelect.btnPnl.add( "button", undefined, "✔ Create New Gallery Pic" );
    btnNewPubPic = wSelect.btnPnl.add( "button", undefined, "✔ Create New Publication Pic" );
    btnMakeContribGradient = wSelect.btnPnl.add( "button", undefined, "✔ Create Contributor Black Gradient" );
    btnMakeTN = wSelect.btnPnl.add( "button", undefined, "✔ Make Thumbnail" );
    btnMakeXS = wSelect.btnPnl.add( "button", undefined, "✔ Make XS Thumbnail" );
    btnMakeWebP = wSelect.btnPnl.add( "button", undefined, "Make WebP" );
    btnChooseInput = wSelect.btnPnl.add( "button", undefined, "✔ Select starting location" );
    btnPrepForGallery = wSelect.btnPnl.add( "button", undefined, "✔ Toy Gallery Format (resize, crop & watermark)" );
    btnLaunchPowerShell = wSelect.btnPnl.add( "button", undefined, "Launch PowerShell" );
    // btntestMe = wSelect.btnPnl.add( "button", undefined, "----- TEST -----" );

    cancelBtn = wSelect.btnPnl.add( "button", undefined, "Cancel", { name: "cancel" } );
  
    // ==========================================================
    // ==================== BUTTON FUNCTIONS ====================
    // ==========================================================
    

    btnLaunchPowerShell.onClick = function(){
        //var spawn = require("child_process").spawn;
        //spawn("C:\Users\214003825\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Windows PowerShell"); // ,[".\download-packages-license.ps1"]
        //%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
        wSelect.close();
        createSmartObject();
        //
    }

    btnPrepForGallery.onClick = function(){

        if(doc.height >= doc.width){
            //alert("portrait or square: YES. H=" + doc.height + ", W=" + doc.width);
            // ----- Portrait Orientation -----
            // Resize Image to fit the height
            wSelect.close();
            createSmartObject();
            var myUnits = "PX";
            var picHeight = 1248;//1248, 874
            doc.resizeImage(null, UnitValue(picHeight, myUnits), null, ResampleMethod.BICUBICSHARPER);
            // Crop Canvas width, from middle-center
            var canvasWidth = 874;
            doc.resizeCanvas(UnitValue(canvasWidth, myUnits),null, AnchorPosition.MIDDLECENTER);
            
            // Add Signature
            makeContribGradient();

        } else {
            alert("landscape: YES. H=" + doc.height + ", W=" + doc.width);
        }

    };

    
    function makeContribGradient(){
        signatureName = prompt("Input Signature Name", "Who?"); // prompt for a layer name
        var layer1 = doc.artLayers.add();// create a new layer
            layer1.name = "BG Gradient";
            layer1.blendMode = BlendMode.NORMAL;
            layer1.opacity = 20;
        var myColor = new SolidColor(); 
            myColor.rgb["hexValue"] = "000000";
        var myTextColor = new SolidColor(); 
            myTextColor.rgb["hexValue"] = "FFFFFF";    
        //doc.selection.selectAll();
        var mySelectionCoords = [ //define a selection, x,y pairs - left to right, top to bottom
            [-500, 800],
            [1200, 800],
            [1200, 1600],
            [-500, 1600]
        ];
        doc.selection.select(mySelectionCoords, undefined, 100, true);// Create the selection
        doc.selection.fill(myColor);// fill the selection        
        
        // >>> Create the SIGNATURE text layer
        var layer2 = doc.artLayers.add();// create a new layer  
            layer2.name = "Signature";  
            layer2.kind = LayerKind.TEXT;
            layer2.textItem.color = myTextColor;
            layer2.textItem.size = 24;
            layer2.textItem.contents = ": " + signatureName;
            layer2.textItem.justification = Justification.CENTER;
            layer2.textItem.position = Array(((app.activeDocument.width / 2)+12), 1200);// how to include 27px for icon width?
            layer2.textItem.capitalization = TextCase.ALLCAPS;
            layer2.textItem.kind = TextType.PARAGRAPHTEXT;
        var myUnits = "PX";
        var iconXPosition = ((UnitValue(app.activeDocument.width, myUnits) / 2) - (UnitValue(layer2.textItem.width, myUnits) / 2) + 6);
        // >>> Create the ICON text layer
        var layer3 = doc.artLayers.add();// create a new layer  
            layer3.name = "Instagram Icon";  
            layer3.kind = LayerKind.TEXT;
            layer3.textItem.color = myTextColor;
            layer3.textItem.size = 24;
            layer3.textItem.contents = '\uf16d ';
            layer3.textItem.font = "FontAwesome5Brands-Regular";
            layer3.textItem.justification = Justification.CENTER;
            //layer3.textItem.position = layer2.textItem.position; // works sorta
            layer3.textItem.position = Array(UnitValue(iconXPosition, myUnits), 1201);
            layer3.textItem.convertToShape();
        wSelect.close();
        doc.selection.deselect();
        // =========== SAVE FOR WEB ===============
        saveForWebAsJpeg();
        //makeTN();
        //makeXS();

    }

    btnMakeContribGradient.onClick = function(){
       makeContribGradient();    
    };
    
    btnNewPubPic.onClick = function(){
        
        // create a new document
        var myFile = app.documents.add(524,679);
        app.activeDocument = myFile; // make the file active
        wSelect.close();
        
    };

    btnNewGalleryPic.onClick = function(){
        
        // create a new document
        var myFile = app.documents.add(874,1248);
        app.activeDocument = myFile; // make the file active
        wSelect.close();
        
    };

    btnMakeTN.onClick = function(){
        makeTN();
    };
    
    btnMakeXS.onClick = function(){
        makeXS();
    };
    
    btnMakeWebP.onClick = function(){
        alert('This action might be broken :(');
        
        wSelect.close();
        var directory = Folder.selectDialog('Where would you like to save the file?');
        doc.saveAs(new File(directory + "/_output_PS.webp"), undefined);
        
    };

    btnChooseInput.onClick = function(e) {
        
        var directory = Folder.selectDialog('Choose the folder of source images');
        // if (directory) inputBox.text = directory;
        alert(directory);
        
    };

    // ==========================================================
    // ====================== FUNCTIONS =========================
    // ==========================================================
    function createSmartObject() {
        var idnewPlacedLayer = stringIDToTypeID( 'newPlacedLayer' );
        executeAction(idnewPlacedLayer, undefined, DialogModes.NO);
    }
    function makeTN(){
        wSelect.close();
        createSmartObject();
        var tnWidth = 298;
        var myUnits = "PX";
        doc.resizeImage(UnitValue(tnWidth, myUnits), null, null, ResampleMethod.BICUBICSHARPER);

        //saveForWebAsJpeg(app.activeDocument, 6);
    }
    function makeXS(){
        wSelect.close();
        createSmartObject();
        var xsWidth = 100;
        var myUnits = "PX";
        doc.resizeImage(UnitValue(xsWidth, myUnits), null, null, ResampleMethod.BICUBICSHARPER);

        //saveForWebAsJpeg(app.activeDocument, 6);
    }
    
    // function chooseOutput(e) {
    //     // Choose where to save files
    //    var directory = Folder.selectDialog('Choose the output folder');
    //     // if (directory) outputBox.text = directory;
    //  }

    function saveJPEG(doc, saveFile, q) {
        
        var saveOptions = new JPEGSaveOptions( );
        saveOptions.embedColorProfile = true;
        saveOptions.formatOptions = FormatOptions.STANDARDBASELINE;
        saveOptions.matte = MatteType.NONE;
        saveOptions.quality = q; 
        doc.saveAs( saveFile, saveOptions, true );
        
        /*
        saveJPEG( app.activeDocument, new File(decodeURI(app.activeDocument.path)+'/sample.jpg'), 10 );
        var doc = app.activeDocument;
        var docName = doc.name;docName = docName.match(/(.*)(\.[^\.]+)/) ? docName = docName.match(/(.*)(\.[^\.]+)/):docName = [docName, docName];var suffix = '_300';
        var saveName = new File(decodeURI(doc.path)+'/'+docName[1]+suffix+'.jpg');
        saveJPEG( app.activeDocument, saveName, 10 );
        */
    }
    //    saveJPEG( app.activeDocument, new File('~/Desktop/sample.jpg'), 10 );

    function saveForWebAsJpeg(file) {
        toyName = prompt("Input TOY Name", "SomeToyName_01-6_"); // prompt for a layer name
       
        var saveOptions = new ExportOptionsSaveForWeb();
            saveOptions.format = SaveDocumentType.JPEG;
            saveOptions.quality = 6;
        var myDirectory;
            switch (saveWhere) {
                case "XS":
                    //var myDirectory =
                    break;
                case "TN":
                    //var myDirectory = 
                    break;
                case "TOY":
                    //var myDirectory = 
                    break;    
                case "CHOOSE":
                    myDirectory = Folder.selectDialog('Choose the output folder');
                    break; 
                case "SAME":// SAME ORIGINAL FOLDER
                    //myDirectory = File((app.activeDocument.path) + '/TEST.jpg');
                    if(!signatureName){
                        signatureName = "photographer";
                    }
                    //myDirectory = File((doc.path) + '/by_' + signatureName + '_via_instagram'+'.jpg');
                    myDirectory = File((doc.path) + '/' + toyName + 'by_' + signatureName + '_via_instagram'+'.jpg');
                    break;        
                default:
                    // SAME ORIGIN FOLDER
                    if(!signatureName){
                        signatureName = "photographer";
                    }
                    myDirectory = File((doc.path) + '/' + toyName + 'by_' + signatureName + '_via_instagram'+'.jpg');
                    //D:\_CODE_PRACTICE\__webp-files-test
            }
        //alert(myDirectory);
        //app.activeDocument.exportDocument(myDirectory, ExportType.SAVEFORWEB, saveOptions);
       /* 
        var saveForWebOptions = new JPEGSaveOptions( );
            saveForWebOptions.embedColorProfile = true;
            saveForWebOptions.formatOptions = FormatOptions.STANDARDBASELINE;
            saveForWebOptions.matte = MatteType.NONE;
            saveForWebOptions.quality = 6;
        */
        // doc.saveAs( saveFile, saveForWebOptions, true );
        // doc.saveAs(new File(myDirectory + "/_TEST"), saveForWebOptions);
        //doc.SaveDocumentType(ExportOptionsSaveForWeb, mySaveOptions);
        var myHistorySavePoint = doc.activeHistoryState;
        doc.flatten();
        doc.exportDocument(myDirectory, ExportType.SAVEFORWEB, saveOptions);
        doc.activeHistoryState = myHistorySavePoint;// Revert to unflattened!
        
        signatureName = undefined;// Clear the var!
        //doc.close(SaveOptions.DONOTSAVECHANGES);
    
    }

    function saveForWeb(doc) { // other param?: saveFile
        
        var saveForWebOptions = new JPEGSaveOptions( );
        saveForWebOptions.embedColorProfile = true;
        saveForWebOptions.formatOptions = FormatOptions.STANDARDBASELINE;
        saveForWebOptions.matte = MatteType.NONE;
        saveForWebOptions.quality = 6; 
        // doc.saveAs( saveFile, saveForWebOptions, true );
        doc.exportDocument();
        
    }
    
    function makeLayer(theName){
        
        app.activeDocument = myFile; // make the file active
        var layer1 = myFile.artLayers.add();// create a new layer
        layer1.name = theName;
        layer1.blendMode = BlendMode.NORMAL;
        
    }

    function valueResizeTN(){
        
        var tnWidth = 298;
        var myTNUnits = "PX";
        doc.resizeImage(UnitValue(tnWidth, myTNUnits), null, null, ResampleMethod.BICUBICSHARPER);
        wSelect.close();

        // short size or long size?
        // if(doc.width >= doc.height){
        //     doc.resizeImage(UnitValue(tnWidth, myUnits), null, null, ResampleMethod.BICUBICSHARPER);
        // } else {
        //     doc.resizeImage(UnitValue(tnWidth, myUnits), null, null, ResampleMethod.BICUBICSHARPER);
        // }
        
    }

    function valueResizeWebP(units){
        alert('yes! ' + units);
    }


// ------------------------------------
    wSelect.show();
}
main();
