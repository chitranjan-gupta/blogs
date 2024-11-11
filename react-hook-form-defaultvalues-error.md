---
tags:
  - node
  - npm
  - react
  - nextjs
  - react-hook-form
  - zod
description: React Hook Form defaultValues does not reset automatically after first rendering
slug: react-hook-form-defaultvalues-error
published: "true"
created: 2024-10-21T11:51:00
updated: 2024-10-21T11:51:00
categories:
  - React
  - Programming
  - React Hook Form
  - Nextjs
title: React Hook Form - defaultValues not changing
---
### Example of Full Component:

```tsx
import { IconBadge } from "@/components/icon-badge";
import { LayoutDashboard, ArrowLeft } from "lucide-react";
import { CareerForm } from "./_components/career-form";
import Link from "next/link";
import { useEffect, useState } from "react";
import Loader from "@/components/loader";
import { Actions } from "./_components/actions";
import { getCareer } from "@/core/query";
import { Career } from "@/core/types";

const CareerIdPage = ({ params }: { params: { companyId: string; careerId: string } }) => {
  const { companyId, careerId } = params;
  const [refresh, setRefresh] = useState<boolean>(false);
  const { data: career, error } = getCareer(companyId, careerId);

  // Handling loading or error states
  if (!career && !error) {
    return <Loader />;
  }
  if (error) {
    return <div>Error loading career data: {error.message}</div>;
  }

  // Define required fields for career setup completion
  const requiredFields = [career.title]; // Expand this as needed
  const totalFields = requiredFields.length;
  const completedFields = requiredFields.filter(Boolean).length;
  const completionText = `(${completedFields}/${totalFields})`;
  const isComplete = requiredFields.every(Boolean);

  return (
    <div className="p-6">
      <Link href={`/admin/companies/${companyId}/careers`} className="flex items-center text-sm hover:opacity-75 transition mb-6">
        <ArrowLeft className="h-4 w-4 mr-2" /> Back to careers
      </Link>
      <div className="flex items-center justify-between">
        <div className="flex flex-col gap-y-2">
          <h1 className="text-2xl font-medium">Career Setup</h1>
          <span className="text-sm text-slate-700">
            Complete all fields {completionText}
          </span>
        </div>
        <div>
          <Actions career={career} setRefresh={setRefresh} />
        </div>
      </div>
      <div className="w-full">
        <div className="flex items-center gap-x-2">
          <IconBadge icon={LayoutDashboard} />
          <h2 className="text-xl">Customize your career</h2>
        </div>
        <CareerForm initialData={career} setRefresh={setRefresh} />
      </div>
    </div>
  );
};

export default CareerIdPage;
```

```tsx
"use client";

import * as z from "zod";
import axios from "axios";
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import toast from "react-hot-toast";
import { Dispatch, SetStateAction, Fragment } from "react";

// Importing form section components
import { InputForm } from "./input-form";
import { BenefitsForm } from "./benefits-form";
import { RequirementsForm } from "./requirements-form";
import { ResponsibilitiesForm } from "./responsibilities-form";
import { SkillsForm } from "./skills-form";

// Importing types and constants
import { Career } from "@/types";
import { form_attrs, formSchema } from "./form-type";

interface CareerFormProps {
  initialData: Career;
  setRefresh: Dispatch<SetStateAction<boolean>>;
}

export const CareerForm = ({ initialData, setRefresh }: CareerFormProps) => {
  // Initialize useForm hook with validation and default values
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: initialData,
  });

  const { isSubmitting, isValid, handleSubmit, errors } = form.formState;

  // onSubmit function to handle form submission
  const onSubmit = async (values: z.infer<typeof formSchema>) => {
    try {
      // Sending PATCH request to update career data
      await axios.patch(
        `${process.env.NEXT_PUBLIC_API_URL}/api/companies/${initialData.companyId}/careers/${initialData.id}`,
        values,
        {
          withCredentials: true,
          headers: { "Content-Type": "application/json" },
        }
      );

      // Success message and trigger refresh
      toast.success("Career updated");
      setRefresh((prev) => !prev);
    } catch (error: any) {
      // Error handling
      toast.error("Something went wrong");
    }
  };

  // Function to render the corresponding form section based on the attribute
  const renderFormSection = (form_attr: string) => {
    const formProps = { form }; // Assuming all form sections need access to the `form` object

    switch (form_attr) {
      case "responsibilities":
        return <ResponsibilitiesForm {...formProps} />;
      case "benefits":
        return <BenefitsForm {...formProps} />;
      case "requirements":
        return <RequirementsForm {...formProps} />;
      case "skills":
        return <SkillsForm {...formProps} />;
      default:
        return <InputForm {...formProps} />;
    }
  };

  return (
    <div className="flex flex-row flex-wrap gap-x-5 gap-y-5">
      <form onSubmit={handleSubmit(onSubmit)}>
        {/* Dynamically render form sections */}
        {form_attrs.map((form_attr) => (
          <Fragment key={form_attr}>
            {renderFormSection(form_attr)}

            {/* Display error messages if validation fails */}
            {errors[form_attr] && (
              <p className="text-red-500 text-sm">
                {errors[form_attr]?.message}
              </p>
            )}
          </Fragment>
        ))}

        {/* Submit button */}
        <button type="submit" disabled={isSubmitting || !isValid}>
          {isSubmitting ? "Submitting..." : "Submit"}
        </button>
      </form>
    </div>
  );
};
```


> but the problem is when the careerid is changed and new data is fetched but the defaultValue of useForm is not changing

Ah, I see! You're encountering an issue where the `defaultValue` of the `useForm` hook in `react-hook-form` isn't updating when the `careerId` changes and new data is fetched. This is a common issue because `useForm` only accepts its `defaultValues` once, during the initial render, and doesn't automatically update when the data changes after the initial mount.

To fix this, you need to explicitly reset the form when the new career data arrives. React Hook Form provides a `reset` method, which can be used to update the form's values with new data.

Here’s how you can handle this:

### Solution: Use `reset` to Update Form Data Dynamically

1. **Use `useEffect` to Watch for `initialData` Changes**: Whenever `initialData` (your fetched career data) changes, you can call the `reset` method to update the form with the new data.

2. **Update `useForm` with the New Data**: You’ll need to call `form.reset(initialData)` inside a `useEffect` that listens for changes in `initialData` (your fetched data). This ensures the form's values are reset whenever new data is fetched, even if the `careerId` changes.    

### Updated Code with `reset` Logic:
```ts
"use client";
import * as z from "zod";
import axios from "axios";
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import { Dispatch, SetStateAction, Fragment, useEffect } from "react";
import toast from "react-hot-toast";
import { Career } from "@/types";
import { form_attrs, formSchema } from "./form-type";
import { BenefitsForm } from "./benefits-form";
import { RequirementsForm } from "./requirements-form";
import { ResponsibilitiesForm } from "./responsibilities-form";
import { SkillsForm } from "./skills-form";

interface CareerFormProps {
  initialData: Career;
  setRefresh: Dispatch<SetStateAction<boolean>>;
}

export const CareerForm = ({ initialData, setRefresh }: CareerFormProps) => {
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: initialData,
  });
// This is to be added
  useEffect(() => {
    form.reset(initialData); // Reset form when initialData changes
  }, [initialData, form]);
//
  const { isSubmitting, isValid, handleSubmit, formState: { errors } } = form;

  const onSubmit = async (values: z.infer<typeof formSchema>) => {
    try {
      await axios.patch(
        `${process.env.NEXT_PUBLIC_API_URL}/api/companies/${initialData.companyId}/careers/${initialData.id}`,
        values,
        {
          withCredentials: true,
          headers: { "Content-Type": "application/json" },
        }
      );
      toast.success("Career updated");
      setRefresh((prev) => !prev);
    } catch (error: any) {
      console.error(error);
      toast.error(error.response?.data?.message || "Something went wrong");
    }
  };

  const renderFormSection = (form_attr: string) => {
    const formProps = { ...form };  // Assuming formProps contains the necessary data for all form sections

    switch (form_attr) {
      case "responsibilities":
        return <ResponsibilitiesForm {...formProps} />;
      case "benefits":
        return <BenefitsForm {...formProps} />;
      case "requirements":
        return <RequirementsForm {...formProps} />;
      case "skills":
        return <SkillsForm {...formProps} />;
      default:
        return <InputForm {...formProps} />;
    }
  };

  return (
    <div className="flex flex-row flex-wrap gap-x-5 gap-y-5">
      <form onSubmit={handleSubmit(onSubmit)}>
        {form_attrs.map((form_attr) => (
          <Fragment key={form_attr}>
            {renderFormSection(form_attr)}
            {errors[form_attr] && (
              <p className="text-red-500 text-sm">{errors[form_attr]?.message}</p>
            )}
          </Fragment>
        ))}
        <button type="submit" disabled={isSubmitting || !isValid}>
          {isSubmitting ? "Submitting..." : "Submit"}
        </button>
      </form>
    </div>
  );
};
```

### Explanation of Changes:

1. **`useEffect` with `form.reset`**:
    - `useEffect` is watching the `initialData` prop for changes.
    - When `initialData` changes (for example, when the `careerId` changes and new data is fetched), the form is reset with `form.reset(initialData)` to update the form fields with the new data.
2. **Calling `form.reset`**:
    - This updates the form with new values when the career data changes, even after the initial mount. This ensures the form correctly reflects the latest data when a new career is selected.

### Key Points:

- **`form.reset(initialData)`**: Resets the form to the new `initialData`, ensuring that the form reflects the updated values.
- **`useEffect` Dependency**: The effect runs every time the `initialData` changes, which triggers the form reset with the new data.

### Important Notes:

- Make sure that `initialData` is not `null` or `undefined` when you call `form.reset`. You may want to add a check or a loading state if `initialData` is initially empty before fetching.
- If you're updating data frequently, be careful with the performance implications of calling `form.reset` repeatedly. However, for most use cases, this is fine and ensures that the form stays in sync with the state.

With this approach, the form will automatically update with the new `initialData` whenever it changes, solving the issue where the form doesn't reflect updated data when the `careerId` changes. 

#### Refrences:
1. https://react-hook-form.com/docs/useform/reset
