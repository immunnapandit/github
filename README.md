# github

[DataContract]
public class UpdateAddressLocationContract
{
    CustAccount custAccount;
    LogisticsLocationId locationId;

    [DataMemberAttribute]
    public CustAccount parmCustAccount(CustAccount _custAccount = custAccount)
    {
        custAccount = _custAccount;
        return custAccount;
    }

    [DataMemberAttribute]
    public LogisticsLocationId parmLocationId(LogisticsLocationId _locationId = locationId)
    {
        locationId = _locationId;
        return locationId;
    }
}



public class UpdateAddressLocationService
{
    public void updateAddressLocation(UpdateAddressLocationContract contract)
    {
        CustTableEntity custTableEntity;
        CustTable custTable;

        ttsBegin;
        select forUpdate custTable
            where custTable.AccountNum == contract.parmCustAccount();

        if (custTable)
        {
            custTableEntity = CustTableEntity::find(custTable.RecId);
            if (custTableEntity)
            {
                custTableEntity.LocationId = contract.parmLocationId();
                custTableEntity.update();
            }
        }
        ttsCommit;
    }
}



public class UpdateAddressLocationController extends SysOperationServiceController
{
    public static void main(Args _args)
    {
        UpdateAddressLocationContract contract = new UpdateAddressLocationContract();
        contract.parmCustAccount("CUST001");
        contract.parmLocationId("NEW_LOCATION_ID");

        SysOperationServiceController controller = new SysOperationServiceController();
        controller.startOperation(classnum(UpdateAddressLocationService), methodstr(UpdateAddressLocationService, updateAddressLocation), contract);
    }
}


internal final class TestClassess
{
    // write a code of sysoperation framework for updating the customer delivery addess to a custom table fields

    public static void updateCustomerAddress()
    {
        UpdateAddressLocationController::main(null);
    }
}
