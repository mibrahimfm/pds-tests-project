# pds-tests-project
Documenta testes para a matéria PDS no semestre 2021/2

1 - **Sistema**: microsoft/PowerToys
   **Teste**: SaveChanges()
   **Classe**: FancyZonesSettingsTests.cs
   **Código do teste**:
        
    private void SaveChanges()
    {
        string isEnabled = _saveButton.GetAttribute("IsEnabled");
        Assert.AreEqual("True", isEnabled);

        _saveButton.Click();

        isEnabled = _saveButton.GetAttribute("IsEnabled");
        Assert.AreEqual("False", isEnabled);
    }   
    

   O que é testado: Valida se o botão está disponível inicialmente, e se ele se torna indisponível a partir do momento que ele é clicado

2 - **Sistema**: microsoft/PowerToys
   **Teste**: SettingsOpenWithContextMenu ()
   **Classe**: PowerToysTrayTests
   **Código do teste**:

    public void SettingsOpenWithContextMenu()
    {

        //open tray

        trayButton.Click();

        WaitSeconds(1);

        isTrayOpened = true;


        //open PowerToys context menu

        AppiumWebElement pt = PowerToysTrayButton();

        Assert.IsNotNull(pt);

        new Actions(session).MoveToElement(pt).ContextClick().Perform();

        //open settings

        session.FindElementByXPath("//MenuItem[@Name=\"Settings\"]").Click();


        //check settings window opened

        WindowsElement settingsWindow = session.FindElementByName("PowerToys Settings");

        Assert.IsNotNull(settingsWindow);


        isSettingsOpened = true;
    }

O que é testado: Valida se a classe PowerToysTrayButton está sendo corretamente incializada e se o Windows elemento pode ser inicializa buscando pelo elemento PowerToysSettings

3) **Sistema**: microsoft/PowerToys
   **Teste**: CreateSplitter()
   **Classe**:  FancyZonesEditorGridZoneResizeTests.cs
   **Código do teste**: 
   
        public void CreateSplitter()
        {
            OpenCreatorWindow("Columns", "EditTemplateButton");
            WindowsElement gridEditor = session.FindElementByClassName("GridEditor");
            Assert.IsNotNull(gridEditor);
            ReadOnlyCollection<AppiumWebElement> zones = gridEditor.FindElementsByClassName("GridZone");
            Assert.AreEqual(3, zones.Count, "Zones count invalid");

            const int defaultSpacing = 16;
            int splitPos = zones[0].Rect.Y + zones[0].Rect.Height / 2;

            new Actions(session).MoveToElement(zones[0]).Click().Perform();

            zones = gridEditor.FindElementsByClassName("GridZone");
            Assert.AreEqual(4, zones.Count);

            //check splitted zone 
            Assert.AreEqual(defaultSpacing, zones[0].Rect.Top);
            Assert.IsTrue(Math.Abs(zones[0].Rect.Bottom - splitPos + defaultSpacing / 2) <= 2);
            Assert.IsTrue(Math.Abs(zones[1].Rect.Top - splitPos - defaultSpacing / 2) <= 2);
            Assert.AreEqual(Screen.PrimaryScreen.Bounds.Bottom - defaultSpacing, zones[1].Rect.Bottom);
        }
        
O que é testado: Valida a criação de uma nova “zona” no GridZone, a partir da criação de um splitter.
