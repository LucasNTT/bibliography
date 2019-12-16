Assuming json file exists
{"items": [
    {
		"Code": "CP05",
		"Title": "FFT Algorithms using Discrete Weighted Transforms",
		"Authors": [{"First_Name": "Richard E.", "Last_Name": "Crandall"}, {"First_Name": "Corell", "Last_Name": "Fagin"}],
		"Filename": "FFT Algorithms.pdf",
		"Tags": "CP05,1991,paper,8-pages,FFT,Crandall,Fagin,Algorithm,Discrete,Weighted,Transforms"},
    {
        "Name": "Sam",
        "Points": 65,
        "age": 21}
]}


dans HTML
    <script type="text/javascript">
		$('#tags').inputTags({
			minLength: 2,
			change:function($elem) {
				DocViewer.OnTagChanged();
			}
		});
        $(document).ready(function () {
            $.ajax({
				url: "data/users.json",
				dataType: "json",
				success: function(response) {
					//console.log(response.Users);
				    DocViewer.Init(response);
					DocViewer.Refresh();
				}
			});
        });
    </script>

Dans JS

function DocViewer() { };
DocViewer.Data = null;
DocViewer.noPage = 1;
DocViewer.tags = [];

DocViewer.Init = function (data) {
    DocViewer.Data = data;
};

DocViewer.OnTagChanged = function () {
	DocViewer.tags = [];
	alert($('#tags').tags);
	//foreach (var tag in tags) { // to be review according inputTags
	//	DocViewer.tags[DocViewer.tags.length] = tag;
	//}
};

DocViewer.Refresh = function () {
    var html = 'No data available.';
    if (DocViewer.Data.length > 0) {

        $('h1').html(
            DocViewer.Data[DocViewer.Data.length - 1]['Nm_Last_Name'] +
            ' ' +
            DocViewer.Data[DocViewer.Data.length - 1]['Nm_First_Name']
        );
		
        html = '<table id="doc">';
        html += '<thead>';
        html += '<tr>';
        for (var cat in DocViewer.Param['Categories']) {
            if (DocViewer.Param['Categories'][cat]['NbVisibleCol'] > 0) {
                html += '<th colspan="' + DocViewer.Param['Categories'][cat]['NbVisibleCol'] + '" class="group">' + DocViewer.Param['Categories'][cat]['Caption'] + '</th>';
            }
        }

        html += '</tr>';
        html += '<tr>';
        for (var m in DocViewer.Param.VisibleCol) {
            if (DocViewer.Param.VisibleCol[m].Visible) {
                html += '<th>' + DocViewer.Param.VisibleCol[m].Caption + '</th>';
                colCount++;
            }
        }
        html += '</tr>';
        html += '</thead>';
        html += '</table>';
        document.getElementById('dataHeader').innerHTML = html;    
	}
};


Pour récupérer les paramètres: 
function getParameterByName(name, url) {
    if (!url) url = window.location.href;
    name = name.replace(/[\[\]]/g, '\\$&');
    var regex = new RegExp('[?&]' + name + '(=([^&#]*)|&|#|$)'),
        results = regex.exec(url);
    if (!results) return null;
    if (!results[2]) return '';
    return decodeURIComponent(results[2].replace(/\+/g, ' '));
}
// Can be "" if present but empty, and null is abstent

Paramètres
- pour le numéro de page
- pour les tags

Tout changement de tag fait un ràz du numéro de page
On peut changer la virgule par un espace, mais dans ce cas quand il tapera une seule lettre on aura l'erreur. Pourquoi pas, à tester...

// Il faut limiter les tags à A..Z, a..z, 0..9, -, _, ' + les caractères accentués
pattern = "[A-z0-9À-ž \-_]+"


