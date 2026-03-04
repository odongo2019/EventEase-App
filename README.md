@page "/registration/{EventName}"
@using System.ComponentModel.DataAnnotations

<h2>Register for @EventName</h2>

<EditForm Model="@registrationModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <div class="mb-3">
        <label>Name:</label>
        <InputText class="form-control" @bind-Value="registrationModel.Name" />
        <ValidationMessage For="@(() => registrationModel.Name)" />
    </div>

    <div class="mb-3">
        <label>Email:</label>
        <InputText type="email" class="form-control" @bind-Value="registrationModel.Email" />
        <ValidationMessage For="@(() => registrationModel.Email)" />
    </div>

    <button type="submit" class="btn btn-success">Register</button>
</EditForm>

@if (registrationSuccess)
{
    <div class="alert alert-success mt-3">
        Registration successful for @registrationModel.Name!
    </div>
}

@code {
    [Parameter] public string EventName { get; set; }

    private RegistrationModel registrationModel = new RegistrationModel();
    private bool registrationSuccess = false;

    private void HandleValidSubmit()
    {
        registrationSuccess = true;

        // Save to session or state management for tracking
        UserSession.AddRegistration(EventName, registrationModel);
    }

    public class RegistrationModel
    {
        [Required(ErrorMessage = "Name is required")]
        public string Name { get; set; }

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email format")]
        public string Email { get; set; }
    }
}
