<script lang="ts">
  import { error } from "@sveltejs/kit";

  // initial form data
  let formData: App.UserFormData = {
    firstName: "",
    lastName: "",
    email: "",
    password: "",
  };

  //initial error state
  let errors: App.FormErrors = {};

  // flag/ boolean to check if the form has been submitted
  let isSubmitted = false;

  let submittedData: Omit<App.UserFormData, "password"> | null = null;

  const emailRegex = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/;

  function formValidation(): boolean {
    const newErrs: App.FormErrors = {};
    let isValid = true;

    //first name validation
    if (!formData.firstName && formData.firstName.length > 3) {
      newErrs.firstName =
        "First name is required and should be longer than 3 characters";
      isValid = false;
    }
    //last name validation
    if (!formData.lastName && formData.lastName.length > 3) {
      newErrs.lastName =
        "Last Name is required and should be longer than 3 characters";
      isValid = false;
    }
    //email validation
    if (!formData.email && formData.email.length > 6) {
      newErrs.email =
        "Email is required and should be longer than 6 characters";
      isValid = false;
    } else if (!emailRegex.test(formData.email)) {
      newErrs.email = "Invalid email format";
      isValid = false;
    }

    if (!formData.password) {
      newErrs.lastName = "Password is required";
      isValid = false;
    } else if (formData.password.length < 6) {
      newErrs.password = "Password should be at least 6 characters";
      isValid = false;
    }
    errors = newErrs;
    return isValid;
  }

  function handleSubmit(Event: Event) {
    Event.preventDefault();

    if (formValidation()) {
      submittedData = {
        firstName: formData.firstName,
        lastName: formData.lastName,
        email: formData.email,
      };
      isSubmitted = true;

      //reset form

      formData = {
        firstName: "",
        lastName: "",
        email: "",
        password: "",
      };
      errors = {};
    }
  }

  function handleInput(field: keyof App.UserFormData) {
    if (errors[field]) {
      errors[field] = undefined;
    }
    if (isSubmitted) {
      isSubmitted = false;
    }
  }

  // TODO: Implement form state management
  // TODO: Implement form validation
  // TODO: Implement submit handler
  // TODO: Implement success state management
</script>

<div class="form-container">
  <form on:submit={handleSubmit}>
    <div class="form-group">
      <label for="firstName">First Name</label>
      <input
        id="firstName"
        bind:value={formData.firstName}
        class:error={errors.firstName}
        on:input={() => handleInput("firstName")}
        type="text"
        placeholder="Enter your first name"
      />
      {#if errors.firstName}
        <span class="error-message">{errors.firstName}</span>
      {/if}
    </div>

    <div class="form-group">
      <label for="lastName">Last Name</label>
      <input
        id="lastName"
        bind:value={formData.lastName}
        class:errors={errors.lastName}
        on:input={() => handleInput("lastName")}
        type="text"
        placeholder="Enter your last name"
      />
      {#if errors.lastName}
        <span class="error-message">{errors.lastName}</span>
      {/if}
    </div>

    <div class="form-group">
      <label for="email">Email</label>
      <input
        id="email"
        type="email"
        bind:value={formData.email}
        class:error={errors.email}
        on:input={() => handleInput("email")}
        placeholder="Enter your email"
      />
      {#if errors.email}
        <span class="error-message">{errors.email}</span>
      {/if}
    </div>

    <div class="form-group">
      <label for="password">Password</label>
      <input
        id="password"
        bind:value={formData.password}
        class:error={errors.email}
        on:input={() => handleInput("password")}
        type="password"
        placeholder="Enter your password"
      />
      {#if errors.password}
        <span class="error-message">{errors.password}</span>
      {/if}
    </div>

    <button type="submit" class="submit-button">Submit</button>
  </form>

  <!-- TODO: Add success message section here -->
  {#if submittedData}
    <div class="success-message">
      <ul>
        <li>First Name: {submittedData.firstName}</li>
        <li>Last Name: {submittedData.lastName}</li>
        <li>
            Email: {submittedData.email}
        </li>
    </ul>
    </div>
  {/if}
</div>

<style>
  .form-container {
    max-width: 480px;
    margin: 0 auto;
    padding: 2rem;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .form-group {
    margin-bottom: 1.5rem;
  }

  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 500;
    color: #374151;
  }

  input {
    width: 100%;
    padding: 0.75rem;
    border: 1px solid #d1d5db;
    border-radius: 6px;
    font-size: 1rem;
    transition: border-color 0.15s ease-in-out;
  }

  input:focus {
    outline: none;
    border-color: #3b82f6;
    box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
  }

  input::placeholder {
    color: #9ca3af;
  }

  input.error {
    border-color: #ef4444;
  }

  .error-message {
    display: block;
    margin-top: 0.5rem;
    font-size: 0.875rem;
    color: #ef4444;
  }

  .submit-button {
    width: 100%;
    padding: 0.75rem 1.5rem;
    background-color: #3b82f6;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.15s ease-in-out;
  }

  .submit-button:hover {
    background-color: #2563eb;
  }

  .submit-button:focus {
    outline: none;
    box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.5);
  }

  .submit-button:disabled {
    background-color: #9ca3af;
    cursor: not-allowed;
  }

  .success-message {
    margin-top: 1.5rem;
    padding: 1rem;
    background-color: #edf7ed;
    border: 1px solid #c8e6c9;
    border-radius: 6px;
    color: #1b5e20;
  }
</style>
