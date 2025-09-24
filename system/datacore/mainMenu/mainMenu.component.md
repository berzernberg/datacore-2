# Main Menu Component

```jsx
function MainMenuComponent() {
    const tabs = [
        { 
            id: 'tab1', 
            label: 'Dashboard',
            componentPath: 'system/datacore/mainMenu/tab1.component.md',
            headerName: 'Tab 1 Component'
        },
        { 
            id: 'tab2', 
            label: 'Notes & Queries',
            componentPath: 'system/datacore/mainMenu/tab2.component.md',
            headerName: 'Tab 2 Component'
        },
        { 
            id: 'tab3', 
            label: 'Analytics',
            componentPath: 'system/datacore/mainMenu/tab3.component.md',
            headerName: 'Tab 3 Component'
        }
    ];
    
    // Simple click handler for tab switching
    const handleTabClick = async (tabId) => {
        // Update active tab styling
        document.querySelectorAll('.tab-button').forEach(btn => {
            btn.classList.remove('active');
        });
        const activeButton = document.querySelector(`[data-tab="${tabId}"]`);
        if (activeButton) {
            activeButton.classList.add('active');
        }
        
        // Load and render the new tab content
        const contentContainer = document.querySelector('.tab-content-container');
        if (contentContainer) {
            contentContainer.innerHTML = '<div class="loading">Loading...</div>';
            
            try {
                const tab = tabs.find(t => t.id === tabId);
                if (tab) {
                    const componentModule = await dc.require(
                        dc.headerLink(tab.componentPath, tab.headerName)
                    );
                    
                    // Get the component function from the module
                    const componentName = tab.headerName.replace(/\s+/g, '');
                    const TabComponent = componentModule[componentName];
                    
                    if (TabComponent) {
                        // Execute the component function to get JSX
                        const componentResult = TabComponent();
                        
                        // Create a temporary div to hold the component
                        const tempDiv = document.createElement('div');
                        tempDiv.innerHTML = '';
                        
                        // Since we can't use React.render, we'll display the component content
                        // This is a simplified approach - in a real DataCore environment,
                        // the component rendering would be handled differently
                        contentContainer.innerHTML = `
                            <div class="tab-content">
                                <p>Component loaded: ${tab.label}</p>
                                <p>Path: ${tab.componentPath}</p>
                                <div class="component-placeholder">
                                    <em>Component content will be rendered here by DataCore</em>
                                </div>
                            </div>
                        `;
                    } else {
                        contentContainer.innerHTML = `<div class="error">Component function not found: ${componentName}</div>`;
                    }
                } else {
                    contentContainer.innerHTML = '<div class="error">Tab configuration not found</div>';
                }
            } catch (error) {
                contentContainer.innerHTML = `<div class="error">Error loading tab: ${error.message}</div>`;
            }
        }
    };
    
    // Load the first tab content after component renders
    setTimeout(() => {
        handleTabClick('tab1');
    }, 100);
    
    return (
        <div className="main-menu-container">
            <div className="tab-navigation">
                {tabs.map(tab => (
                    <button
                        key={tab.id}
                        className={`tab-button ${tab.id === 'tab1' ? 'active' : ''}`}
                        data-tab={tab.id}
                        onClick={() => handleTabClick(tab.id)}
                    >
                        {tab.label}
                    </button>
                ))}
            </div>
            <div className="tab-content-container">
                <div className="tab-content">
                    <h3>Loading...</h3>
                    <div className="content-section">
                        <p>Initializing main menu...</p>
                    </div>
                </div>
            </div>
            <style>{`
                .main-menu-container {
                    width: 100%;
                    max-width: 800px;
                    margin: 0 auto;
                    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
                }
                
                .tab-navigation {
                    display: flex;
                    border-bottom: 2px solid #e1e5e9;
                    margin-bottom: 20px;
                    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                    border-radius: 8px 8px 0 0;
                    padding: 0;
                    overflow: hidden;
                }
                
                .tab-button {
                    flex: 1;
                    padding: 12px 20px;
                    border: none;
                    background: transparent;
                    color: rgba(255, 255, 255, 0.8);
                    cursor: pointer;
                    font-size: 14px;
                    font-weight: 500;
                    transition: all 0.3s ease;
                    position: relative;
                }
                
                .tab-button:hover {
                    background: rgba(255, 255, 255, 0.1);
                    color: white;
                }
                
                .tab-button.active {
                    background: rgba(255, 255, 255, 0.2);
                    color: white;
                    font-weight: 600;
                }
                
                .tab-button.active::after {
                    content: '';
                    position: absolute;
                    bottom: 0;
                    left: 0;
                    right: 0;
                    height: 3px;
                    background: white;
                }
                
                .tab-content-container {
                    background: white;
                    border-radius: 0 0 8px 8px;
                    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
                    min-height: 400px;
                    padding: 24px;
                }
                
                .tab-content {
                    animation: fadeIn 0.3s ease-in;
                }
                
                @keyframes fadeIn {
                    from { opacity: 0; transform: translateY(10px); }
                    to { opacity: 1; transform: translateY(0); }
                }
                
                .tab-content h3 {
                    margin: 0 0 16px 0;
                    color: #2c3e50;
                    font-size: 24px;
                    font-weight: 600;
                }
                
                .content-section {
                    color: #555;
                    line-height: 1.6;
                }
                
                .loading {
                    background: #f8f9fa;
                    border: 1px solid #e9ecef;
                    border-radius: 6px;
                    padding: 16px;
                    margin: 16px 0;
                    border-left: 4px solid #667eea;
                    color: #6c757d;
                    font-style: italic;
                }
                
                .error {
                    background: #f8d7da;
                    border: 1px solid #f5c6cb;
                    border-radius: 6px;
                    padding: 16px;
                    margin: 16px 0;
                    border-left: 4px solid #dc3545;
                    color: #721c24;
                }
            `}</style>
        </div>
    );
}

return { MainMenuComponent };
```