## [Raizlabs Android Best Practices](id:tableOfContents)<a name="tableOfContents"></a>

[JavaDoc](#javadoc)

[Constants](#constants)

[Static Constant Classes vs Enums](#classvsenum)

[ViewHolders](#viewholders)

[Logging](#logging)

[Fragments](#fragments)

[Activities](#activities)

[Fragments vs Activities](#fragmentsvsactivities)

Use text styles / Button styles / Shared dimens etc

***
### [JavaDoc](id:javadoc)<a name="javadoc"></a> 
- Provide Java Doc comments for any objects or methods which are publicly visible and are likely to be used by another developer.
	- Exception: Trivial getters/setters
- **Never** include your name or user name. Source control will handle attribution.
- You must provide:
    - Summary
    - @param descriptions (unless trivially named)
    - @return description
    - Explanations of any thrown exceptions
- Use @link tags to reference classes and methods.
- Use @see tags to note related methods.
- If a parameter is allowed to be null (optional parameter), note "May be null" at the end of the @param description. 
- Make sure to note any assumptions or foreseen edge cases.
			
			/**
			 * Returns the smaller value of the two given values.
			 * @see #max(int, int)
			 * @param val1 The first value to check.
			 * @param val2 The second value to check.
			 * @return The smaller value, or val1 if both are equal.
			 */
			public static int min(int val1, int val2) {
				return (val1 <= val2) ? val1 : val2;
			}
			

[back to top](#tableOfContents)

### [Constants](id:constants)<a name="constants"></a>
- **ALWAYS** use string resources for user-facing strings.
- Define **all** Intent extra keys, Fragment argument keys, FragmentManager tags, etc as final Strings.
- Move constants into the most general class for their usage.
	- Break them out into their own file if they are very general purpose (i.e. used across the app) or there are many of them.
	
[back to top](#tableOfContents)

### [Static Constant Classes vs Enums](id:classvsenum)<a name="classvsenum"></a>
- In general, prefer static classes. Java enums are heavy-handed and are not very portable

			public class Foo {
				public static class Action {
					public static final String ADD = "add";
					public static final String EDIT = "edit";
				}
				
				...
			}
	- When using static classes, make sure relevant methods declare the class where the constants are defined.
	
			public class Foo {
				
				...
				
				/**
				 * Performs the given action as defined by {@link Action}.
				 * @param A value of {@link Action} which defines that to perform.
				 */
				public void performAction(String action) {
					...
				}
			}
		
	- Use enums where type enforcement is more important than portability.
	- Keep in mind that Java enums allow definining methods per value. Prefer enums over decision trees/switches based on static class constants, though portability may make this not an option.
	
[back to top](#tableOfContents)

### [ViewHolders](id:viewholders)<a name="viewholders"></a>
- Use the ViewHolder pattern whenever a class is hanging on to multiple views with the same lifetime.
	- I.E. in a fragment where the all child views are created and destroyed 	simultaneously.
	- This allows us to consolidate all view lookups and to quickly let go of all references to the views when we need to.
- Generally, the ViewHolder constructor should take the root view and populate its members.
- The ViewHolder should exist within the view lifetime and be destroyed when the views are.
	- Example:
	
			public class MyFragment extends Fragment {

				private ViewHolder viewHolder;
	
				@Override
				public void onViewCreated(View view, Bundle savedInstanceState) {
					super.onViewCreated(view, savedInstanceState);
		
					viewHolder = new ViewHolder(view);
					viewHolder.Title.setText(R.string.text);
				}
	
				@Override
				public void onDestroyView() {
					super.onDestroyView();
		
					viewHolder = null;
				}
	
				private static class ViewHolder {
					TextView Title;
					TextView Subtitle;
					ImageView Icon;
		
					public ViewHolder(View view) {
						Title = (TextView) view.findViewById(R.id.title);
						Subtitle = (TextView) view.findViewById(R.id.subtitle);
						Icon = (ImageView) view.findViewById(R.id.image);
					}
				}
			}

[back to top](#tableOfContents)

### [Logging](id:logging)<a name="logging"></a>
- Logging should be used for debugging purposes, but most logging should generally be removed in production builds.
	- Android will automatically clear lower log levels in production builds, but prefer to use a wrapper around Log methods so that you can keep your own control over log levels.
- Log any time unexpected things happen or errors occur.
	- If an exception was caught, be sure to include it in the log statement.
	
			try {
				...
			} catch (Exception e) {
				Log.e(getClass().getSimpleName(), "Exception while reading foo", e);
			}
- Always log using appropriate levels so logging may be stripped accordingly.
- Always use informative tags which help to track down where the statement came from.
	- Class names generally work well.

[back to top](#tableOfContents)

### [Fragments](id:fragments)<a name="fragments"></a> 
- **DO NOT** use constructors.
	- Provide static newInstance(...) style creation methods.
	- Perform setup in onCreate(). 
- Make sure to follow the [ViewHolder](#viewholders) pattern.
- onCreateView() should simply inflate the layout.
- All view lookups, ViewHolder logic, and (generally) data loading should be contained in onViewCreated().
- ViewHolders should be nulled in onDestroyView().
- Properties and data should generally be kept in the arguments bundle.
	- Create getters and setters which wrap these for legibility and ease of use.
	- Pass data from creation methods via these arguments.
	
			private static final String ARG_ITEM_ID = "ItemId";
	
			public static MyFragment newInstance(int itemId) {
				MyFragment fragment = new MyFragment();
				fragment.setItemId(itemId);
				return fragment;
			}
	
			private int getItemId() { return getArguments().getInt(ARG_ITEM_ID); }
			private void setItemId(int id) { getArguments().putInt(ARG_ITEM_ID, id); }
	- Note: Generally the above example will crash since the arguments bundle is null. We typically use a base class for fragments which ensures this bundle is set at construction to circumvent this.
	
[back to top](#tableOfContents)

### [Activities](id:activities)<a name="activities"></a>
- Use mostly as "Fragment Controllers".
	- Even if this means just wrapping another Fragment so as to allow it to be accessed via Intents.
- If arguments are expected from internal app code, provide getIntent() methods which take the arguments and provide a properly constructed Intent.
	- Not necessary if no arguments are expected.
	- Break these methods out into an IntentFactory class if they are very general or abstract and shouldn't rely on the particular activity.
		- e.g. A "navigation" intent which may be handled internally or externally.
		
[back to top](#tableOfContents)

### [Fragments vs Activities](id:fragmentsvsactivities)<a name="fragmentsvsactivities"></a>
- Use fragments as controllers for a set of views operating in the same context or on the same data.
- Use fragments whenever the contents may need to be moved or recontextualized.
	- e.g. Tablet layouts, operating on the same type of data in a different view
- Prefer views over fragments for simple compound controls or controllers.
- Use Activities whenever we may want to link to this view directly from another part of the app. i.e. good candidates for Intents.
	- e.g. Deep linking, showing details of certain items that are accessible in different places.
		- In these cases, it is usually best to write a simple wrapper Activity which simply wraps a Fragment with all the logic, as this Fragment may need to be used individually in other places.

[back to top](#tableOfContents)