Das ist der fertige Code vom Marchzinsberechner:

```csharp
namespace Marchzins_Berechner
{
    public partial class Form1 : Form
    {
        private bool wurdeBerechnet = false;
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            if (wurdeBerechnet)
            {
                MessageBox.Show("Die Berechnung wurde bereits durchgeführt.");
                return;
            }

            DateTime kaufdatum, verkaufsdatum;
            double kapitalBerechnung, zinssatzProzent;

            string inputDateFormat = "dd.MM.yyyy";

            if (!DateTime.TryParseExact(kaufDatum.Text, inputDateFormat, CultureInfo.InvariantCulture, DateTimeStyles.None, out kaufdatum) ||
                !DateTime.TryParseExact(verkaufsDatum.Text, inputDateFormat, CultureInfo.InvariantCulture, DateTimeStyles.None, out verkaufsdatum))
            {
                MessageBox.Show("Ungültiges Datumsformat. Bitte geben Sie das Datum im Format 'dd.MM.yyyy' ein.");
                return;
            }

            if (kaufdatum >= verkaufsdatum)
            {
                MessageBox.Show("Kaufdatum muss vor dem Verkaufsdatum liegen");
                return;
            }

            if (!double.TryParse(kapital.Text, out kapitalBerechnung) || !double.TryParse(zinsSatz.Text, out zinssatzProzent))
            {
                MessageBox.Show("Ungültige Eingabe für Kapital oder Zinssatz");
                return;
            }

            if (kapitalBerechnung < 0 || zinssatzProzent < 0)
            {
                MessageBox.Show("Kapital und Zinssatz dürfen nicht negativ sein");
                return;
            }

            // Umwandlung Zinssatz von Prozent in Dezimalzahl
            double zinssatz = zinssatzProzent / 100.0;

            // Berechnung Anzahl Tage zwischen Kauf- und Verkaufsdatum
            int tage = (verkaufsdatum - kaufdatum).Days;

            // Berechnung Marchzins
            double marchzins = kapitalBerechnung * zinssatz * (tage / 365.0);

            // Anzeige Ergebnis
            marchZins.Text = $"{marchzins.ToString("C", System.Globalization.CultureInfo.CreateSpecificCulture("de-CH"))}";

            anzeigeBox.AppendText($"Kaufdatum: {kaufDatum.Text}" + Environment.NewLine);
            anzeigeBox.AppendText($"Verkaufsdatum: {verkaufsDatum.Text}" + Environment.NewLine);
            anzeigeBox.AppendText($"Kapital: {kapital.Text}" + Environment.NewLine);
            anzeigeBox.AppendText($"Zinssatz: {zinsSatz.Text}" + Environment.NewLine);
            anzeigeBox.AppendText($"Marchzins: {marchzins.ToString("C", System.Globalization.CultureInfo.CreateSpecificCulture("de-CH"))}" + Environment.NewLine);

            wurdeBerechnet = true;
        }

        private void button2_Click(object sender, EventArgs e)
        {
            wurdeBerechnet = false;
            kaufDatum.Clear();
            verkaufsDatum.Clear();
            kapital.Clear();
            zinsSatz.Clear();
            marchZins.Clear();
            anzeigeBox.Clear();
        }

        private void button3_Click(object sender, EventArgs e)
        {
            try
            {
                SaveFileDialog saveFileDialog = new SaveFileDialog();
                saveFileDialog.Filter = "Textdateien (*.txt)|*.txt|Alle Dateien (*.*)|*.*";
                saveFileDialog.Title = "Datei speichern";

                if (!wurdeBerechnet)
                {
                    MessageBox.Show("Es wurde noch keine Berechnung durchgeführt. Es gibt nichts zu speichern.", "Warnung", MessageBoxButtons.OK, MessageBoxIcon.Warning);
                    return;
                }

                try
                {

                }
                catch (Exception ex)
                {
                    MessageBox.Show($"Fehler beim Speichern der Datei: {ex.Message}", "Fehler", MessageBoxButtons.OK, MessageBoxIcon.Error);
                }

                if (saveFileDialog.ShowDialog() == DialogResult.OK)
                {
                    string filePath = saveFileDialog.FileName;

                    using (StreamWriter writer = new StreamWriter(filePath))
                    {
                        writer.WriteLine($"Kaufdatum: {kaufDatum.Text}");
                        writer.WriteLine($"Verkaufsdatum: {verkaufsDatum.Text}");
                        writer.WriteLine($"Kapital: {kapital.Text}");
                        writer.WriteLine($"Zinssatz: {zinsSatz.Text}");
                        writer.WriteLine($"Marchzins: {marchZins.Text}");
                    }

                    MessageBox.Show("Datei wurde erfolgreich gespeichert.", "Erfolg", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Fehler beim Speichern der Datei: {ex.Message}", "Fehler", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void anzeigeBox_TextChanged(object sender, EventArgs e)
        {

        }

        private async void api_Click(object sender, EventArgs e)
        {
            var client = new HttpClient();
            var request = new HttpRequestMessage
            {
                Method = HttpMethod.Get,
                RequestUri = new Uri("https://api.api-ninjas.com/v1/interestrate?country=Switzerland"),
                Headers =
                {
                    { "X-Api-Key", "kiMmYcZ0KHhQS7U2PVyrsQ==Jf6pvyrAxPJkYB5n" },
                },
            };
            
            try
            {
                using (var response = await client.SendAsync(request))
                {
                    response.EnsureSuccessStatusCode();
                    var body = await response.Content.ReadAsStringAsync();

                    int pos = body.IndexOf("rate_pct");
                    body = body.Substring(pos + 11, 4);

                    zinsSatz.Text = body;
                }

            }
            catch 
            {
                MessageBox.Show("Error");
            }
        }
    }
}
```
