# SFDC
Salesforce Api
# SFDC
Salesforce Api

Code to Create Matadata Custom Fields:

@using SFDC

        SalesForceApi api = new SalesForceApi();
        var loginInfo = api.Login("abc@m.com", "********");

        ElementModel model = new ElementModel()
        {
            FullName = "Account.CustomFieldTest3__c",
            Type = FieldType.Url,
            Label = "CustomTest",
        };

        MetadataApi meta = new MetadataApi(loginInfo.sessionId,loginInfo.metadataServerUrl);
        UpsertResult[] result = meta.CreateCustomField(model);
        
        
Custom Objects :

        CustomObject co = new CustomObject();
        co.fullName = model.FullName.Replace(' ', '_') + "__c";
        co.deploymentStatus = DeploymentStatus.Deployed;
        co.deploymentStatusSpecified = true;
        co.description = model.Description;
        co.enableActivities = true;
        co.label = model.Label;
        co.pluralLabel = model.Label + "s";
        co.sharingModel = SharingModel.ReadWrite;
        co.sharingModelSpecified = true;

        co.fields = new CustomField[]{};
        co.nameField = new CustomField();
        co.nameField.label = model.Label;
        co.nameField.type = FieldType.Text;
        co.nameField.typeSpecified = true;
        UpsertResult[] result2 = meta.UpsertCustomObject(co);
Create/Update an actual Opportunity/Contact

       SalesForceApi api2 = new SalesForceApi("endPointUrl","sessionId");


        CreateSObject opportunity = new CreateSObject()
        {
            Type = "Opportunity",
            Fields = new List<ElementField>()
            {
                new ElementField()
                {
                    Key = "Name",
                    Value = "My New Opportunity Name"
                },
                new ElementField()
                {
                    Key = "Amount",
                    Value = "999"
                },
            }
        };

        SFDC.soapApi.SaveResult[] saveResults = api2.CreateElement(opportunity);


        UpdateSObject contact = new UpdateSObject()
        {
            Id = "0031W0000213yCzQAI",
            Type = "Contact",
            Fields = new List<ElementField>()
            {
                new ElementField()
                {
                    Key = "FirstName",
                    Value = "Otto"
                },
                new ElementField()
                {
                    Key = "LastName",
                    Value = "Jespersen"
                },
            }
        };

        SFDC.soapApi.SaveResult[] saveResults = api2.UpdateElement(contact);
