# Main Menu Component

```jsx
function MainMenuComponent() {
    // Define available tabs
    const tabs = [
        { id: 'tab1', label: 'Dashboard' },
        { id: 'tab2', label: 'Notes & Queries' },
        { id: 'tab3', label: 'Analytics' },
        { id: 'tab4', label: 'Custom Tab 4' }
    ];
    
    // Load tab components directly using dc.require
    const loadTabComponent = async (tabId) => {
        try {
            const componentPath = `system/datacore/mainMenu/tabs/${tabId}.component.md`;
            const headerName = `Tab ${tabId.slice(-1)} Component`;
            const componentModule = await dc.require(dc.headerLink(componentPath, headerName));
            const componentName = headerName.replace(/\s+/g, '');
            return componentModule[componentName];
        } catch (error) {
            console.error(`Error loading ${tabId}:`, error);
            return null;
        }
    };
    
    // Pre-load all tab components
    const [Tab1Component, Tab2Component, Tab3Component, Tab4Component] = await Promise.all([
        loadTabComponent('tab1'),
        loadTabComponent('tab2'),
        loadTabComponent('tab3'),
        loadTabComponent('tab4')
    ]);
    
    const tabComponents = {
        tab1: Tab1Component,
        tab2: Tab2Component,
        tab3: Tab3Component,
        tab4: Tab4Component
    };
    
    // State for active tab
    let activeTab = 'tab1';
    
    // Get current tab content
    const getCurrentTabContent = () => {
        const TabComponent = tabComponents[activeTab];
        if (TabComponent) {
            return TabComponent();
        }
        return (
            <div className="error">
                <h3>Tab Not Found</h3>
                <p>Could not load content for {activeTab}</p>
            </div>
        );
    };
    
    return (
        <div className="main-menu-container">
            <div className="tab-navigation">
                {tabs.map(tab => (
                    <button
                        key={tab.id}
                        className={`tab-button ${tab.id === activeTab ? 'active' : ''}`}
                        data-tab={tab.id}
                        onClick={() => {
                            activeTab = tab.id;
                            // Force re-render by updating the content
                            const container = document.querySelector('.tab-content-container');
                            if (container && tabComponents[activeTab]) {
                                // Update active button styling
                                document.querySelectorAll('.tab-button').forEach(btn => {
                                    btn.classList.remove('active');
                                });
                                document.querySelector(`[data-tab="${activeTab}"]`).classList.add('active');
                                
                                // This is a simplified approach - in DataCore, you'd typically
                                // trigger a re-render of the component instead
                                container.innerHTML = `
                                    <div class="tab-content">
                                        <p><strong>Active Tab:</strong> ${tab.label}</p>
                                        <p><em>Tab component loaded successfully!</em></p>
                                        <div class="component-info">
                                            <h4>Component Details:</h4>
                                            <ul>
                                                <li>Tab ID: ${activeTab}</li>
                                                <li>Component: ${tabComponents[activeTab] ? 'Loaded' : 'Not Found'}</li>
                                                <li>Path: system/datacore/mainMenu/tabs/${activeTab}.component.md</li>
                                            </ul>
                                        </div>
                                    </div>
                                `;
                            }
                        }}
                    >
                        {tab.label}
                    </button>
                ))}
            </div>
            <div className="tab-content-container">
                {getCurrentTabContent()}
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
                
                .component-info {
                    background: #f8f9fa;
                    border: 1px solid #e9ecef;
                    border-radius: 6px;
                    padding: 16px;
                    margin: 16px 0;
                    border-left: 4px solid #28a745;
                }
                
                .component-info h4 {
                    margin: 0 0 8px 0;
                    color: #495057;
                }
                
                .component-info ul {
                    margin: 0;
                    padding-left: 20px;
                }
                
                .component-info li {
                    margin: 4px 0;
                    color: #6c757d;
                }
            `}</style>
        </div>
    );
}

return { MainMenuComponent };
```